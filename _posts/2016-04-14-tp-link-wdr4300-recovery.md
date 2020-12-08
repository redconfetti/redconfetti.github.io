---
layout: post
title: Unbricking TP-Link TL-WDR4300
categories:
- Linux
tags:
- wifi
- tp-link
- wdr4300
- openwrt
- mesh
---

I tried to flash the TP-Link TL-WDR4300 router with a custom OpenWRT image
recently, and after doing so I was unable to connect to the device like I
expected.

Here is how you can recover / un-brick the device.
<!--more-->

## Install tftpd

I'm using an Ubuntu machine. There are instructions for installing tftpd and
xinetd, but these don't seem to work with Ubuntu 14.04. If you're using Windows,
then I recommend [this video](https://www.youtube.com/watch?v=1hWT35w6sVI).

I found instructions on a [Spiceworks.com forum](https://community.spiceworks.com/how_to/100006-install-and-configure-tftp-under-ubuntu-14-04)
that worked for me.

### Install tftpd-hpa and tftp client

```shell
sudo apt-get install tftpd-hpa tftp
```

### Edit the TFTPd Configuration File

Edit `/etc/default/tftpd-hpa` to use the following configuration

```shell
sudo vi /etc/default/tftpd-hpa
```

```ini
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/tftpboot"
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS="-s -c -l -vv"
```

The `-vv` will cause the TFTP server to log out to /var/log/syslog

### Create the TFTP Directory

The server will serve files from the `/tftpboot` directory. You need to create
it and give it the proper permissions.

```shell
sudo mkdir /tftpboot
sudo chmod -R 777 /tftpboot
sudo chown -R nobody /tftpboot
```

### Restart the service

```shell
sudo service tftpd-hpa restart
```

### Test the TFTP Daemon

Create a file in the directory

```shell
echo "this is a test" > /tftpboot/test.txt
```

Download the file via tftp to your home directory. If it downloads fine, your
tftp server is running.

```shell
$ cd ~
$ tftp localhost
tftp> get test.txt
Received 16 bytes in 0.1 seconds
tftp> quit
$ cat test.txt
this is a test
```

## Download the Latest Firmware

Download the firmware from the [tp-link official support page] to the `/tmp`
directory and unzip the file.

```shell
cd /tmp
wget http://www.tp-link.us/res/down/soft/TL-WDR4300_V1_151104_US.zip
unzip TL-WDR4300_V1_151104_US.zip
```

Copy the binary image file to `/tftpboot` named as ``.

```shell
cp wdr4300v1_en_us_3_14_3_up_boot\(151104\).bin /tftpboot/wdr4300v1_tp_recovery.bin
```

[tp-link official support page]: http://www.tp-link.us/download/TL-WDR4300.html#Firmware

## Configure your Machine

Change your machines IP address to `192.168.0.66`. The router will try to
connect to this address and download the image from it in a future step. This
requires a subnet setting of `255.255.255.0`, with a gateway of `0.0.0.0`.

## Enable TFTP Recovery Mode

Taken from [OpenWRT - TP-Link TL-WDR4300 - Flashing via TFTP](https://wiki.openwrt.org/toh/tp-link/tl-wdr4300#flashing_via_tftp):

Power up the TL-WDR4300 and as soon as the asterisk/star symbol to the right of
the power/IO image starts to flash, hold down the WPS/Reset button that is on
the back of the device for about 10 seconds. The asterisk symbol should begin
to flash much faster than before. Let the device continue to fit and do this
until it reboots.

Wait for the firmware transfer (about 20s), firmware flash (about 90s) and
subsequent reboot (about 30s).

You can tail the syslog to see if the router is actually interfacing with the
TFTPd server.

```shell
tail -F /var/log/syslog
```

## References

* [DD-WRT Forum - How to unbrick TP-Link N750 WDR-4300](http://www.dd-wrt.com/phpBB2/viewtopic.php?t=278435)
* [DD-WRT - TP-Link TL-WRT4300](http://www.dd-wrt.com/wiki/index.php/TP-Link_TL-WDR4300#Restoring_Stock_Firmware)
