# hamclock-systemd

This project creates a Debian package to install a compiled version of
[HamClock](https://www.clearskyinstitute.com/ham/HamClock/) and enable
launch on boot using [systemd](https://www.freedesktop.org/wiki/Software/systemd/)

## Installation - APT Package

The package includes a precompiled version of HamClock built for the
Raspberry Pi frame buffer fb0 800x480 pixes which matches the Raspberry Pi
7 inch touch display.

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
package repository to your system.

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