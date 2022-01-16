---
title: "Fedora Linux Install Guide"
date: 2021-07-09
categories:
- linux
tags: 
- guides
draft: false

---

This is a guide for how to install Fedora 34 onto your system. 

<!--more-->

## Backup 

Please ensure that your current system is backed up. The guide will not be
showing a way to install Fedora alongside your current system. Though it is
possible, many have found it to be a less than ideal experience. Assume that all
the data on the disk you are going to install Fedora to is going to be lost.
Backing up is not enough. If you have spare drives available, ensure that you
can restore from the backup onto the new drive without risk to your existing
drive. 

I highly recommend getting a SATA SSD of at least 500GB. It is affordable and
plenty for a linux-based OS. Having a separate drive to test drive a linux
install allows you to work with a real bare-metal installation without risking
losing your daily-driver machine. 

Some might choose to run an installation on a virtual machine. Though that is
good to try out the installation of linux, the performance penalties (for some
Virtual Machine software without graphics hardware acceleration) and driver
struggles makes it difficult to recommend as a long term solution. 

## Creating an installation media
Get a USB flash disk that is at least 8GB. Most people have 1 lying around.
Again, check that there is no important information in the drive. 

Go to the [Fedora](https://getfedora.org/en/workstation/download/) download
page. For those using Windows or MacOS, you can just get the "Fedora Media
Writer" software and just follow the instructions to create a USB Fedora
installation media. 

For those using Linux, you have quite a number of choices. 
Firstly, download the iso image for x86\_64. It has the name "x86\_64 DVD ISO".
You can then choose either of the following options:

1. Using the dd in the terminal

For the more technically oriented and terminal-saavy users. 

- take note of the name of your USB drive
    - the name should be "/dev/sdX"

```shell
$ lsblk
```

- install to your drive
  - "/path/to/image.iso" is the path to your downloaded fedora
  iso image
  - "/dev/sdX" is the name of your device from lsblk
  - all other arguments are as is

```shell
$ sudo dd if=/path/to/image.iso of=/dev/sdX bs=8M status=progress oflag=direct
```


2. Using gnome disk application
- Open Gnome Disk Utility, usually by searching for "Disks" application in your
  linux applications
- Click on the correct disk on the left side lists of disks
- Click on the 3 dot options menu on the top right
- Select the "Restore Disk Image" option
- Browse and select the Fedora image you downloaded
- Restore the image

## Installing Fedora

Now we can get into the real installation process. With your installation disk
attached to the USB, boot / reboot your system and enter into the "Start
Fedora-Workstation-Live" option on the boot screen. Sometimes, your computer
might not boot into the install disk and into your main OS instead. Please go
into the BIOS and change the boot-order.

You will be greeted by a "Welcome to Fedora" screen. Click "Install to Hard
Drive" 

Choose your Language, keyboard layout and Timezone. For me, I usually choose
English (United States) and the US keyboard layout. 

For "Installation Destination", it is not so straightforward as we are going to
be setting up our filesystem with a naming convention that allows us to use an
application called Timeshift. 

For "Storage Configuration", choose "Automatic". Fedora 34 will clear out your
drive and create 3 partitions: FAT for your EFI, a 1GB ext4 for your boot files,
and the remaining space with a single BTRFS partition. Press "Done" on the top
left hand side of the Storage page. Proceed with the installation by pressing
"Begin Installation". 

When done, reboot into your installation media again. This time, we are going to
re-install Fedora. Repeat everything except choosing the "Advanced Custom
(Bivet-GUI)" option in Storage Configuration. 

Ensure that you did the following: 
1. There is an EFI System partition, setting the Mountpoint "/boot/efi"
2. There is an ext4 partition, setting the Mountpoint to "/boot"
3. There is a BTRFS partition, you can give it a name, but do not add a
   Mountpoint
    - You might notice that there might be volumes in the BTRFS partition. If
      they are greyed out and you can't edit them, delete them.
    - Ensure that you have 2 subvolumes with the following name and mountpoints:
      1. name: @, mountpoint: /
      2. name: @home, mountpoint: /home

When done, proceed with the installation. 

Why did I do this? Firstly, setting up the partitions yourself might cause a bug
where the wrong FAT filesystem is chosen for the EFI system partition, causing
you to not be able to boot. Secondly, Fedora default layout minimizes issues
when upgrading to the next Fedora version. Thirdly, the bootloader sets a fixed
name for the BTRFS subvolumes, so if you just decide to change them manually
after the initial install and not re-install as I have done, you will not be
able to boot unless you tweak the GRUB bootloader settings, which I highly
recommend you not to do as it is very hard to troubleshoot. 

## What do you have now?
You now have a vanilla installation of Fedora Linux with the default Gnome
desktop environment. This serves as a very good foundation for future
configurations. You can start exploring how to configure your own system, or
follow my [post-install guide](https://richardwong.xyz/linux/2021/07/10/fedora-post-install-guide/).
