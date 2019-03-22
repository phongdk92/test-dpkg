# This is a debian package test

### 1. Here's your basic source package layout:
```
test-dpkg/
    -- src
    -- helloFriend (script to run our project)
    -- debian/
        -- changelog
        -- copyright
        -- compat
        -- rules
        -- control
        -- install
```  

### 2. For a new project 

Run dch --create in the directory to create a properly formatted `debian/changelog` entry.

```
dch --create
```
The file `debian/changelog` is created with the following content
```
PACKAGE (VERSION) UNRELEASED; urgency=medium

  * Initial release. (Closes: #XXXXXX)

 -- phongdk <phongdk@ubuntu.local>  Fri, 22 Mar 2019 15:01:38 +0700
```
We have to define it **PACKAGE** and **VERSION**
```
test-dpkg (0.1) UNRELEASED; urgency=medium
```

### 3. Other files in `debian/` are described as follows:

- debian/copyright

```
Format: http://www.debian.org/doc/packaging-manuals/copyright-format/1.0/
Upstream-Name: test-dpkg
Upstream-Contact: phongdk<phongdk@ubuntu.local>

Files: *
Copyright: 2019, phongdk<phongdk@ubuntu.local>
License: (GPL-2+ | LGPL-2 | GPL-3 | whatever)
 Full text of licence.
 .
 Unless there is a it can be found in /usr/share/common-licenses
```

- debina/compat (This should be)

```
7
```

- debian/rules

```
#!/usr/bin/make -f

%:
    dh $@ --with python3
Note that there must be "tab" before dh $@ --with python2, not spaces.
```

- debian/control

```
Source: test-dpkg
Section: python
Priority: optional
Maintainer: phongdk <phongdk@phongdk.local>
Build-Depends: python (>= 2.7-3~)
Standards-Version: 3.9.2
X-Python-Version: >= 2.7


Package: test-dpkg
Architecture: all
Section: python
Depends: python-appindicator, ${misc:Depends}, ${python:Depends}
Description: short description
 A long description goes here.
 It can contain multiple paragraphs
```

- debian/install (This file indicates which file will be installed into which folder.)

```
helloFriend /usr/bin/
src /var/local/test-dpkg
```

### 4. To build a debian package

```
$ cd ~/workspace/test-dpkg
$ debuild --no-tgz-check --no-sign
```

This will create a functional deb package. Lintian is going to throw a few warnings regarding the lack of an orig.tar.gz, but unless you plan on creating a proper upstream project that makes tarball releases you'll probably just want to ignore that for now.

The `test-dpkg_0.1_all.deb` is located at `~/workspace/`

### 5. Install from package
The `test-dpkg_0.1_all.deb` package now can be delivered to other machines and installed.

```
$ sudo dpkg -i test-dpkg_0.1_all.deb
$ sudo apt-get install -f test-dpkg_0.1_all.deb
```

### 6. Test script

```
$ helloFriend Rob
Thứ sáu, 22 Tháng 3 năm 2019 17:09:20 +07
Hello World: @phongdk
Hello Rob
```

### 7. Notes
Since in *debian/control*, it is required to install some dependencies, e.g python-appindicator, we have to install it ahead

```
sudo apt --fix-broken install
```