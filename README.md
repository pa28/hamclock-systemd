# hamclock-systemd -- February 3, 2025

This project creates three Debian packages to install a compiled versions of
[HamClock](https://www.clearskyinstitute.com/ham/HamClock/):
1. hamclock: Provides binaries compiled to use the X11 windows system to display on the desktop.
2. hamclock-systemd: Provides binaries compiled to use the raw frame buffer (/dev/fb0) which
can be started using the include systemd service.
3. hamclock-web: Provides binaries compiled to provide web-only access to hamclock. Like the
repository `hamclock-systemd` these binaries can also be controlled using the included systemd
service.

Starting with build system version 13.1 the compiler and linker options `-g` are removed
eliminating generation of debug symbols wich saves 10 megabytes per package.

## Notes on Running `hamclock-web` Service

The service file is located at `/etc/systemd/system/hamclock-web.service`. By default, it
contains:
```
[Unit]
Description=HamClock web service
After=multi-user.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=on-failure
RestartSec=1
User=root
Group=root
ExecStartPre=/bin/sleep 10
ExecStart=/usr/local/bin/hamclock-web-1600x960

[Install]
WantedBy=multi-user.target
```
You **should** probably change `User` and `Group` to a normal user and group. Using root:root works, and
may be what you want, but it will allow anyone with access to the web port root access to the host
computer.

You can also choose the resolution to use by setting `ExecStart`.

Once these changes are made, start the service (instructions below) and browse to http://localhost:8081/live.html

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

#### Using a systemd enabled package

See the [repository instructions here.](https://pa28.github.io/Repository)

Then perform the following:
```
sudo apt update
```
Install the systemd package desired (or both):
```
sudo apt install hamclock-systemd
```
```
sudo apt install hamclock-web
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

For the web service use similar commands but replace `hamclock` with `hamclock-web`

## Using this Git Repository

To use this repository to build your own package follow these steps:

1. Install prerequisites if not already installed:
```
sudo apt update
sudo apt -y install cmake make g++ libx11-dev
```
2. Clone the repository to your local system (requires git)
```
sudo apt install git # if not already installed.
git clone https://github.com/pa28/hamclock-systemd.git
```

3. Build the packages:
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
