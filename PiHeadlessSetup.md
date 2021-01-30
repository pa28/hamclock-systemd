# Setting up a Raspberry Pi Headless

Requires the ability to mount and edit the SDCard or other drive that will
be used in the Pi. These instructions are for a current Linux desktop PC.
See the links below for other operating systems.

These instruction work on my Linux Mint (Debian derivative) machine. See
the links in each session for more specific instructions.

## SSH

See: https://phoenixnap.com/kb/enable-ssh-raspberry-pi

1. Insert the SDCard. It should mount an display two partitions:
    1. boot - the boot partition
    1. rootfs - the root file system
1. Create a file on the boot partition call ssh:
    ```
    touch /media/$USER/boot/ssh
    ```

## Wifi

See: https://howchoo.com/g/ndy1zte2yjn/how-to-set-up-wifi-on-your-raspberry-pi-without-ethernet

1. Insert the SDCard. It should mount an display two partitions:
    1. boot - the boot partition
    1. rootfs - the root file system
2. Use the plain text file editor of your choice to create a file at
`/media/$USER/boot/wpa_supplicant.conf`
3. Enter the following contents into the file, changing the details to
match your configuration:
```
country=CA # Your 2-digit country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
    ssid="YOUR_NETWORK_NAME"
    psk="YOUR_PASSWORD"
    key_mgmt=WPA-PSK
}
```

## Boot

1. Install the SDCard in the Raspberry Pi and start it up. After resizing the root filesystem 
it will reboot. You should now be able to logon with Username: `pi` and password `raspberry`.

## Change the default username to something other than `pi` (optional)

See: https://thepihut.com/blogs/raspberry-pi-tutorials/how-to-change-the-default-account-username-and-password
And: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/v2v_guide/preparation_before_the_p2v_migration-enable_root_login_over_ssh

1. Logon to the pi (hostname should be `raspberrypi`) as `pi` using SSH `ssh pi@raspberrypi` you have
configured. Become `root`: `sudo su`. Edit the file `/etc/ssh/sshd_config`. Find the line:
`#PermitRootLogin prohibit-password` and change that line to, or enter a new line that is:
`PermitRootLogin yes`. That section of the file should now look like this:
    ```
    # Authentication:
    
    #LoginGraceTime 2m
    #PermitRootLogin prohibit-password
    PermitRootLogin yes
    #StrictModes yes
    #MaxAuthTries 6
    #MaxSessions 10
    ```

1.  Restart the `sshd` service: `systemctl restart sshd`

1.  Give the `root` account a password with the `passwd` command.

1.  Exit from `root` and `pi` accounts. All this is to allow you to logon as `root` since you won't be able
to change the `pi` account name if anything is running under that account.

1.  `ssh` to `raspberrypi` as `root` with the password you just set.

1.  You can change the user name with the following two commands (change `newname` to your username):
    ```
    usermod -l newname pi                   # Change the default account name.
    usermod -m -d /home/newname newname     # Match the home directory to the new default name
    ```

1.  Logout as root. Logon with the new account name.

## Scripting

If you do this a lot, like I do, you may find a script like this helpful:
```
#!/bin/bash
MOUNT="/media/${USER}"
HOSTNAME="smartypi"
USERNAME="your_user_name"

touch ${MOUNT}/boot/ssh

cat << EOF > ${MOUNT}/boot/wpa_supplicant.conf
country=CA # Your 2-digit country code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
network={
    ssid="SSID"
    psk="password"
    key_mgmt=WPA-PSK
}
EOF

echo $HOSTNAME > ${MOUNT}/rootfs/etc/hostname

cat << EOF > ${MOUNT}/rootfs/etc/hosts
127.0.0.1       localhost
::1             localhost ip6-localhost ip6-loopback
ff02::1         ip6-allnodes
ff02::2         ip6-allrouters

127.0.1.1       $HOSTNAME
EOF

cp ${MOUNT}/rootfs/etc/group ${MOUNT}/rootfs/etc/group.old
sed -e "s/^pi:/${USERNAME}:/" < ${MOUNT}/rootfs/etc/group.old | sed -e "s/:pi/:${USERNAME}/" | sed -e "s/,pi/,${USERNAME}/" > ${MOUNT}/rootfs/etc/group

cp ${MOUNT}/rootfs/etc/passwd ${MOUNT}/rootfs/etc/passwd.old
sed -e "s/^pi:/${USERNAME}:/" < ${MOUNT}/rootfs/etc/passwd.old | sed -e "s/\/pi:/\/${USERNAME}:/" > ${MOUNT}/rootfs/etc/passwd

cp ${MOUNT}/rootfs/etc/shadow ${MOUNT}/rootfs/etc/shadow.old
sed -e "s/^pi:/${USERNAME}:/" < ${MOUNT}/rootfs/etc/shadow.old > ${MOUNT}/rootfs/etc/shadow

if [ -d ${MOUNT}/rootfs/home/pi ]
then
        mv ${MOUNT}/rootfs/home/pi ${MOUNT}/rootfs/home/$USERNAME
fi

# Copy ssh credentials for continuity. Assumes tar file exists in the working directory.
tar -xzf ${HOSTNAME}.ssh.tgz -C ${MOUNT}/rootfs/home/${USERNAME}

# Set the local time zone for the system.
sudo ln -s /usr/share/zoneinfo/America/Toronto /etc/localtime
```
