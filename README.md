# hamclock-systemd

This project creates a Debian package to install a compiled version of
[HamClock](https://www.clearskyinstitute.com/ham/HamClock/) and enable
launch on boot using [systemd](https://www.freedesktop.org/wiki/Software/systemd/)

## Running a Pi Headless

Setting up a headless Raspberry Pi is not as straight forward as a regular set up.
You may find the instructins and links in
[PiHeadlessSetup](https://github.com/pa28/hamclock-systemd/blob/main/PiHeadlessSetup.md)
helpful.

## Installation - APT Package

The package includes a precompiled version of HamClock built for the
Raspberry Pi frame buffer fb0 800x480 pixes which matches the Raspberry Pi
7 inch touch display. This is the version that is configured to start.
The package (as of 2.57.2-4) includes all four versions of HamClock for the
frame buffer:
* `hamclock-fb0-800x480`
* `hamclock-fb0-1600x960`
* `hamclock-fb0-2400x1440`
* `hamclock-fb0-3200x1920`

To use a different display size edit `/etc/systemd/system/hamclock.service`
and change the line `ExecStart=/usr/local/bin/hamclock-fb0-800x480` to
start the version that matches your system.

This change makes the package and the installed footprint a little bigger.
Each version is about 4 MB, relative to even a smallish 8 GB SDCard that is not
really significant.

### Install Witout adding the Package Repository.

You can download an APT package from the GitHub repository Releases to your Pi and
install directly from the file:
```
wget https://github.com/pa28/hamclock-systemd/releases/download/v2.57.2.4/hamclock-systemd-2.57.2-4.armv7l.deb
sudo apt install -f ./hamclock-systemd-2.57.2-4.armv7l.deb
```

## Versioning

The package versioning is designed to follow HamClock versioning and
provide information on any changes or updates to the wrapping systemd
code. For example the package: `hamclock-systemd-2.57.1-3` can be
understood as:
* Containing HamClock version 2.57
* systemd code version 1
* Build number 3

## Availability

Some binary packages will be attached to this source repository, however
a better way to get the most current version built would be to add my
package repository to your system. I will try to keep the packages up
to date, but won't necessarily add all new packages to this GitHub
repository.

### Package Repository

Thanks to the people over at [GemFury](https://gemfury.com/) who offer
free public repositories of all kinds I can offer an easy way to 
install HamChrono and keep updated.

### Installation

On your Raspberry Pi or x86 Debian based distribution create this file:
`/etc/apt/sources.list.d/ve3ysh.list`

And insert this line:
`deb [trusted=yes] https://apt.fury.io/ve3ysh/ /`

Then perform the following:
```
sudo apt update
sudo apt install hamclock-systemd
```
To start HamClock:
```
sudo systemctl start hamclock
```
To enable HamClock start on boot:
```
sudo systemctl enable hamclock
```
To stop HamClock if you want to do so:
```
sudo systemctl stop hamclock
```
To prevent HamClock from starting on boot:
```
sudo systemctl disable hamclock
```

## Using this Git Repository

To use this repository to build your own package follow thses steps:

1.  Install prerequisites if not already installed:
```
sudo apt update
sudo apt -y install cmake make g++ libx11-dev
```
2.  Clone the repository to your local system (requires git)
```
sudo apt install git # if not already installed.
git clone https://github.com/pa28/hamclock-systemd.git
```
3.  Download the current version of the HamClock distribution
```
cd hamclock-systemd/SourceCode
rm ESPHamClock.zip # Delete the old version if there is one
wget https://www.clearskyinstitute.com/ham/HamClock/ESPHamClock.zip
unzip ESPHamClock.zip
./buildall
```
4.  The contents of the SourceCode directory should now contain the four supported display size binaries:
```
richard@tardis:~/CLionProjects/hamclock-systemd/SourceCode$ ls -l
total 21292
-rwxrwxr-x 1 richard richard     198 Jan 29 15:19 buildall
drwxr-xr-x 3 richard richard    4096 Jan 30 09:32 ESPHamClock
-rw-rw-r-- 1 richard richard 1161229 Jan 29 12:07 ESPHamClock.zip
-rwxrwxr-x 1 richard richard 4951016 Jan 30 09:32 hamclock-fb0-1600x960
-rwxrwxr-x 1 richard richard 5245984 Jan 30 09:32 hamclock-fb0-2400x1440
-rwxrwxr-x 1 richard richard 5659552 Jan 30 09:32 hamclock-fb0-3200x1920
-rwxrwxr-x 1 richard richard 4770312 Jan 30 09:32 hamclock-fb0-800x480
```
5.  Build the package:
```
cd ..
#
# Edit CMakeLists.txt to set the project version number. For apt install to accept the package as new the
# version number must be greater than a currently installed package.
#
mkdir cmake-build-debug
cd cmake-build-debug
cmake -DCMAKE_BUILD_TYPE=Release ..
make package
```
6.  Output of `make package` should look similar to this:
```
Run CPack packaging tool...
CPack: Create package using DEB
CPack: Install projects
CPack: - Run preinstall target for: hamclock-systemd
CPack: - Install project: hamclock-systemd
CPack: Create package
CPackDeb: - Generating dependency list
CPack: - package: /home/richard/HamClock/cmake-build-release/hamclock-systemd-2.57.2-4.armv7l.deb generated.
```