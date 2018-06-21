---
title: "Sony Vaio Pro 11 and Ubuntu 14.10"
date: 2014-10-26T00:00:00-07:00
draft: false
---

Canonical released Ubuntu 14.10 this week.  The changes over 14.04 are limited but the inclusion of the 3.16 Linux kernel makes it a one-shot install on the SVP11.

## Installation

Go to the Ubuntu releases page and download a 14.10 iso.   Gnome 3 has grown on me so I use UbuntuGnome rather than the default Ubuntu desktop.  Choose the amd64 architecture.  Once your download is completed, you can use a tool like LinuxLive USB Creator to write the iso out to a thumb drive.

Shutdown the laptop and boot by pressing the ASSIST button (located above the F5 keys).  Select to boot from USB media.

The LiveCD will start up.  You can mess around in here and get an idea of what the new release is like.  Since you've decided you love it, go ahead and kick off the installation to disk.  Mash through the defaults.  If you want full disk encryption, and you probably would for a laptop, when you get to the disk layout stage, I would recommend creating two partitions following your Windows NTFS ones.   The first is a boot partition, I use ~300MB.  This just needs to be able to hold your linux-images for booting.  These images are something like 50MB for each kernel you have installed.  You can use ext4 as a filesystem and mount /boot to it.  Next use the remaining space as a "Physical Volume for Encryption".  This will ask you to supply a passphrase, which you will need to enter every single time you boot into Ubuntu.  This is your encryption key, keep it safe but make sure you'll remember it... otherwise you data is scrambled, encrypted and useless.  That is the point of LUKS secured partitions.  Once the volume is made, you'll need to add your mountpoints within it.  I just mount all of "/" underneath this encrypted volume.  That should cover everything, go ahead and proceed through the rest of the installation.  I have my user account automatically log in, since I am the only user on my system and since I'm prompted for the LUKS encryption password at startup anyway.

After the install and reboot, you'll be greeted with GRUB which will send you into Linux, or in the case of dual boot systems, give you a choice which you would like to boot into.  Boot into Linux (Ubuntu in my case).  You'll be prompted for your disk encryption passphrase here at every boot before the system fully comes online.  Real badass.

## Customizations

Here are the things I do each time I get a new Linux system up.

### Oracle Java

You'll want it if you're using NetBeans or JetBrains IDEs since the performance is better than OpenJDK (at the time of writing).  Install Java 8 using Webupd8's PPA with the following commands:

```shell
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

### Intel Linux Graphics

Get the latest stable Intel Linux graphics driver stack fresh from their repo by installing their GUI tool.

### Memory Tweaks

Let's discourage use of the swapfile and try to keep data in memory instead. To do this, we edit the "swappiness". Create a swappiness conf file in /etc/sysctl.d/ containing the line "vm.swappiness=0".  As the super user (sudo doesn't work for this one), you can do this with the following command.:

``` shell
echo "vm.swappiness=0" > /etc/sysctl.d/10-swappiness.conf
```

### Plank Dock

I'm a big fan of Plank Dock.  You can pick up the latest and greatest with the following commands:

```shell
sudo add-apt-repository ppa:ricotz/docky
sudo apt-get update
sudo apt-get install plank
```

### Fan Control

The fan can be set to 3 different speed profiles. To see these profiles, use the following command:

```shell
$ cat /sys/devices/platform/sony-laptop/thermal_profiles
balanced silent performance
```
To control the speed, pipe one of the various speed profiles into the thermal_control using the following format:

```shell
echo "balanced" > /sys/devices/platform/sony-laptop/thermal_control
```
Note you'll need superuser access to make this change.

### Android Apps

One cool thing about the latest versions of Chrome is the ability to use Chrome Apps on your laptop/desktop machine.  I never really found a Calendar program for Linux that I liked... but I sure like Sunrise Calendar on my Android device.   After you've installed Plank, go to the Chrome Webstore.  Now add Sunrise Calendar, or any Chrome app really, to your Chrome environment just as you would on Android.  Launch the app once it's been added.  Now you'll see in Plank that it has spawned off it's own launch icon which you can lock to the dock.  After locking the dock icon, the app will remain there so you can use it like you would any desktop software.

### Additional Software

Here's a list of other software I like to include on my Linux desktop for daily use:

* Calibre - eBook manager
* Moka, Numix - icon and GTK themes, super classy
* VLC player - handles many media files, just in case
* Skype - conference client

## Sources:

* https://wiki.archlinux.org/index.php/Sony_Vaio_Pro_SVP-1x21
* https://dev-nell.com/sony-vaio-pro-11-with-ubuntu.html