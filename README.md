# hamclock-systemd -- June 20, 2021

This project creates a Debian package to install a compiled version of
[HamClock](https://www.clearskyinstitute.com/ham/HamClock/) and enable
launch on boot using [systemd](https://www.freedesktop.org/wiki/Software/systemd/)

### Version 2.58

Version 2.58 of [HamClock](https://www.clearskyinstitute.com/ham/HamClock/) has
deprecated the use of /dev/fb0. It does still build and run.

This version of `hamclock-systemd` installs `hamclock.png` and `hamclock.desktop` in the
[XDG](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html) standard
shared data directory under the program name which on Debian is `/usr/local/share/hamclock/`
though these are not of use unless you build the X-11 versions.

### Version 2.60

As of version 2.60 [HamClock](https://www.clearskyinstitute.com/ham/HamClock/) binaries
that support the use of /dev/fb0 no longer build using the provided source distribuion.
The last successfully built package with thes binaries (Version 2.59) will continue to be
available but new versions will not.

### Versiion 2.63

As of version 2.63 [HamClock](https://www.clearskyinstitute.com/ham/HamClock/) binaries
that support the use of /dev/fb0 are building again and the hamclock-systemd package is
available in my [repository](https://pa28.github.io/Repository).

## Running a Pi Headless

Setting up a headless Raspberry Pi is not as straight forward as a regular set up.
You may find the instructins and links in
[PiHeadlessSetup](https://github.com/pa28/hamclock-systemd/blob/main/PiHeadlessSetup.md)
helpful.

## Installation - APT Package

The package includes a precompiled version of HamClock built for the
Raspberry Pi frame buffer fb0 800x480 pixes which matches the Raspberry Pi
7 inch touch display. This is the version that is configured to start.
The package (as of 2.57.2) includes all four versions of HamClock for the
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

### Install Without adding the Package Repository.

You can download an APT package from the GitHub repository Releases to your Pi and
install directly from the file:
```
wget https://github.com/pa28/hamclock-systemd/releases/download/v2.57.2.4/hamclock-systemd-2.57.2-4.armv7l.deb
sudo apt install -f ./hamclock-systemd-2.57.2-4.armv7l.deb
```

## Versioning

The package versioning is designed to follow HamClock versioning and
provide information on any changes or updates to the wrapping systemd
code. For example the package: `hamclock-systemd-4.11.11.0` can be
understood as:
* Containing HamClock version 4.11
* systemd code version 11
* Build number 0

## Availability

Some binary packages will be attached to this source repository, however
a better way to get the most current version built would be to add my
package repository to your system. I will try to keep the packages up
to date, but won't necessarily add all new packages to this GitHub
repository.

### Package Repository

With the construction and maintenance of an Aptitude Package repository I
decided to deprecate my repository at [GemFury](https://gemfury.com/).

### Installation

#### Using an Aptitude Package -- The Easy Way

See the [instructions here.](https://pa28.github.io/Repository)

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

To use this repository to build your own package follow these steps:

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

3.  Build the packages:
A make package script now takes care of downloading the source code,
updating the version number for CMake, building the eight version of
the program and creating the debian packages.
```
./make_package
```
The ```make_package``` script will exit before attempting to upload the
generated packages to the repository. You can try to remove that but
you won't have the credentials to log into the server, and won't be
on an IP address that the firewall will allow in. If you do and you are
I guess that's on me.