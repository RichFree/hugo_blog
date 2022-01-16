---
title: Introduction to BTRFS
date: 2021-11-09T09:40:05+08:00
categories: 
- linux
tags:
- btrfs
- guides
summary: "Yes, you should even care about the filesystems that you use too."
draft: false

---

BTRFS. B-tree FS. Butter FS. I personally call it B-tree FS. Not many Linux
Distros come with BTRFS by default, but many provide it as an option in the
installer. Personally, I found it to be scary at first, opting to use LVM
(logical volume manager) initially. But with time I realised that the things I
tried to do to ensure redundancy of data was better achieved with BTRFS. 

## Hierarchy and Subvolumes

If you use other filesystems, you would probably set up your filesystem with the
following hierarchy: 

```
boot -> FAT32 / 500MB
swap -> linux-swap / size of RAM
root -> ext4 / 20GB
home -> ext4 / rest of your disk
```

However, in my BTRFS system, it looks like: 

```
boot -> FAT32 / 500MB
BTRFS partition -> rest of the disk 
    root -> @root subvolume 
    home -> @home subvolume
```
This might look very confusing. Firstly, where is the swap? I personally prefer
alternatives to having a swap-partition, be it swap-files or zram. In BTRFS,
creating a disk-based swap is slightly more complicated. We'll touch on this
towards the end of the article. Secondly, why do I not need to set the size of
the root and home partitions in BTRFS? This is your first encounter with
subvolumes: they are not partitions. You can think of them more like directories
than traditional partitions. This means that they dynamically scale with the
size of the files in them. They can expand and shrink without ruining and
corrupting your disk. 

## How to set up a BTRFS disk 

Firstly, we have to wipe your disk to install a new GPT partition layout. 

I assume that you are using a disk named /dev/sda in your lsblk.

We will run fdisk. Run fdisk by running ```fdisk``` in your terminal with
superuser. 

Options: 

- p: print current disk info 
- g: create new GPT table
- n: create new partition
- t: set partition type (this is not setting the filesystem) 
- L: list partition type

For the following steps, just key in the letters as shown.

1. create new table
- g
2. create boot partition
- n
- press enter without keying anything for part number and first sector
- for Last Sector: +500M
- t: 1
3. create linux filesystem partition 
- follow defaults by pressing enter all the way
- t: 20 - reminder: L to list partition types
4. write changes to disk
- w - note: this is irreversible

## Creating filesystems for your partitions

```
lsblk # verify partition layout
mkfs.fat -F32 /dev/sda1
mkfs.btrfs -R free-space-tree -m dup -L <label-name> /dev/sda2
```

some options listed here for mkfs.btrfs are: 
- R : set cache-type. free-space-tree, or space_cache=v2, is a much faster and
  scalable option compared to the default v1
- m : metadata options. single, raid, dup. I used dup for duplicate so that
  there is on disk redundancy for metadata. More on this later. 
- L : label for BTRFS partition. You can give this anything. I usually name this
  after the distro that I install the system on, or the name of the SSD / HDD
  model or manufacturer.

## Create and mount BTRFS subvolumes

I will now demonstrate how to create BTRFS subvolumes. You might need to run
mount and btrfs with superuser.

```
// mount the BTRFS partition on /mnt 
mount -o /dev/sda2 /mnt
cd /mnt

// create the subvolumes 
btrfs subvolume create demo

// let us unmount to remount the individual subvolume
cd /
umount /mnt

// mount the new subvolume "demo"
mount -o noatime,compress=zstd:2,space_cache=v2,subvol=demo /dev/sda2 /mnt
cd /mnt
```

What have I done here? I mounted the BTRFS partition at /mnt, created a
subvolume named "demo", and remounted "demo" at /mnt. 

Note that only the subvolume "demo" is mounted. That means that we can only
access the files in the "demo" subvolume. We can treat "demo" as a fully
contained "partition" in itself. 

This is how we can create "root" and "home" subvolumes (just replace the above
demo with "root" and "home") and mount them at the mountpoints "/" for root and
"/home" for home respectively. 

## Mount options for BTRFS

So far, we have only demonstrated the creation of BTRFS subvolumes that appear
as if they are dynamic partitions. The real magic lies in the options we give to
/etc/fstab to mount them on system boot. 

```
UUID=<UUID> /mountpoint btrfs noatime,compress=zstd:2,ssd,space_cache=v2,subvol=/subvol	0 0
```

Note that we can get UUID using "lsblk -o PATH,UUID"

What are the options listed here? 

- noatime: no atime updates for each file. BTRFS incurs performance penalty with
  frequent atime updates due to its CoW (copy-on-write) nature. 
- compress: set compression algorithm and level of compression. zstd:2 has the
  fastest mix of write and read speeds. zstd:3 is the default and is fine.
  Higher numbers have better compression ratios at the cost of speed. 
    - Refer to
      [here](https://docs.google.com/spreadsheets/d/1x9-3OQF4ev1fOCrYuYWt1QmxYRmPilw_nLik5H_2_qA/edit#gid=0)
      for the benchmarks of different zstd levels and similar compression
      algorithms.
    - as of the time of writing, there is no way to set "fast" levels for ZSTD
      in fstab options ([source](https://github.com/facebook/zstd/issues/2821))
- space_cache=v2 : a scalable free space cache that uses trees. v1 slows down if
  disk is bigger than 1TB
- ssd: tells btrfs we are using SSD. omit this if you use HDD.

## BTRFS features

In the mount options, I have covered the copy-on-write and compression features
of BTRFS. These are some of the amazing features of BTRFS that makes it a modern
filesystem that puts it ahead of other filesystems with the exception of ZFS. 

Copy-on-write, or CoW, is a feature where copy is deferred until the the file is
modified. A copy operation is O(1) time, regardless of the size. That means that
if I were to copy the whole subvolume to a new subvolume (this is called
snapshotting), it is an O(1) operation. Let that sit in for while. 

And speaking of snapshots, you can create read-only snapshots of a live-running
system, allowing you to backup your computer while it is running. And if your
system fails in the future, or a file is lost, you can simply restore it by
booting a live disk, mounting a disk and doing just 2 move operations: ```mv
root broken_root``` and ```mv snapshot root```, then setting the restored root to
read and write and setting the broken root to read-only. Now you have a bootable
system that is identical to the moment when you snapshotted it. I will cover
this in future post on BTRFS snapshots.

Another great feature of snapshots is that you can send them over a unix pipe,
and yes, that includes SSH. So you can remotely snapshot your system and send
the files over the internet to a VPS to store your backups. But sending whole
disks that could be hundreds of gigabytes over the network each time is not
feasible, hence there is a function to perform an incremental send, that is,
sending only the changes of a new snapshot from an older one to build the new
snapshot on the receiving using the existing older snapshot as a base. This is a
great feature that makes backups a breeze and is relatively lighter on network
usage, amounting to only multiple gigabytes for a week of changes. (for some
gigabit internet is not available, but I believe a LAN with another local
machine should be able to achieve hundred megabit speeds with only a couple of
ethernet cables and a relatively modern WiFi router)

Compression in BTRFS, or otherwise known as "transparent compression" is just
lossless compression, aka compression that you benefit from without noticing its
ill-effects (artifacting, data-loss). The obvious benefits of compression is
reduced disk space usage. But the greater benefit of file-system compression is
I/O speed increases. It is sometimes faster to compress data, send the
compressed data through the bottlenecked I/O channels, and decompress the data
on the other side. Here, we utilize the fact that CPUs can compress and
decompress data much faster than the actual I/O transfer speed to sacrifice some
CPU cycles to accelerate I/O. 

In BTRFS, not only is data stored, but metadata is stored alongside data. Every
data has a checksum associated with it, and this checksum is stored in the
metadata. The metadata is important as it is the means to check for data
corruption AND also correct data corruption. Why would there be a need for data
correction? Haven't people been using their computers fine without random data
suddenly disappearing from them, even in the land of Windows? Read
[this](https://arstechnica.com/information-technology/2014/01/bitrot-and-atomic-cows-inside-next-gen-filesystems/)
article. Random file corruptions occur because of bit-rot, and without
checksumming and filesystem-based error-correction, you might lose precious
files to the evil stochastic processes that randomly choose which bits to flip.
With BTRFS and the ```btrfs scrub``` operation, your files will remain the same
for all time, until your disk fails...


## BTRFS quirks

These come mainly from its CoW feature. Remember, CoW means that a copy is
deferred until the file is modified. Why doesn't BTRFS just modify the file in
place? Why keep the old file and write a new file with the modifications?
Doesn't that incur greater disk usage? 

Yes. CoW is not magic. Our O(1) copy is possible because files are never
modified in-place. Our files will always be available by just pointing to it,
thus the new snapshot "copy" simply points back to the unchanged files. Every
modification operation is simply writing a new file at a new block. 

This is the reason why BTRFS subvolumes are always mounted with "noatime"
option. We want to minimize unnecessary file modifications as much as possible,
because each modificaiton will trigger a disk-write operation of the whole file.
This incurs an I/O penalty, but this would be ok if the disk is used without
another BTRFS feature called snapshots because older blocks would simply be
marked as discarded after modification, which in an SSDs case would be trim-ed
in O(1) time. (Note that SSDs also do not modify data in place, but writing new
data to fresh blocks and discarding old blocks, thus BTRFS is very suited for
the architecture of SSDs) The issue lies in using BTRFS with snapshots with
frequent file modifications. A simple recursive atime update through all
directories will trigger a new write on all the updated files without deleting
the old date.  

Disk slow-down will happen in the event of frequent file modifications. Thus
directories or files with frequent modifications like a virtual machine qcow2
file or a database directory will need to have CoW disabled via ```sudo chattr
+C file``` OR ```sudo chattr -R +C dir```. Fortunately, through some magic, it
is possible to snapshot subvolumes with CoW disabled (by momentarily enabling
CoW during the moment of snapshoting to preserve old blocks).

Though BTRFS employs checksumming and utilizes metadata to recover corrupted
data, what happens when the metadata itself is corrupted? In the event that the
metadata is corrupted, perfectly fine data is flagged as corrupted with no
option to tell whether the data is indeed corrupted or the option to restore the
data.  Therefore it is important to have redundancy for the metadata. 

Earlier, the BTRFS partition was created with the "-m dup" option. This creates
a BTRFS partition with duplicate metadata, thus ensuring redundancy even for the
metadata. Now it will take the corruption of 2 duplicate checksums on different
parts of the disk to totally corrupt a data. That's not going to happen unless
your entire disk is failing. 

## Is it worth it? 

BTRFS definitely has its fair share of quirks, but even the best filesystem,
ZFS, has its cons when compared to BTRFS. For a single disk user, BTRFS is
arguably as good as it gets. Now that you have a disk set-up with BTRFs, you can
now follow my other guides tagged with "BTRFS" to unlock even more powerful
functionality. 

