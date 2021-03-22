# Installing the Repository

There is an `aptitude` package that will make it somewhat easier to set up access to my package repository from which you can install the software.
There are two packages:
* `ve3ysh-repo_2.59.5_amd64.deb` for Intel/AMD architecture running a Debian derived Linxu OS;
* `ve3ysh-repo_2.59.5_armhf.deb` for Arm architecture running a Debian derived Linux OS such as Raspberry Pi OS.

## Installation

The following commands will install the `/etc/apt/sources.list.d/ve3ysh.list` file. The contents are:
```
# Repository with HAM Radio related software writtent by VE3YSH
# This file is managed by apt package.
# See: https://github.com/pa28/hamclock-systemd
deb [trusted=yes] https://apt.fury.io/ve3ysh/ /
```

### Intel/AMD

Execute the following commands:
```
wget https://github.com/pa28/hamclock-systemd/releases/download/v2.59.5/ve3ysh-repo_2.59.5_amd64.deb
sudo apt install -f ./ve3ysh-repo_2.59.5_amd64.deb
```
### Arm 

Execute these commands:
```
wget https://github.com/pa28/hamclock-systemd/releases/download/v2.59.5/ve3ysh-repo_2.59.5_armhf.deb
sudo apt install -f ./ve3ysh-repo_2.59.5_armhf.deb
```

## Installing Packages from the Repository

### HamClock systemd

To install the `fb0` targeted versions with the `systemd` (only available for ARM architecture) support files execute:

`sudo apt install hamclock-systemd`

### HamClock X11

To install the `X11` targeted versions execute:

`sudo apt install hamclock`