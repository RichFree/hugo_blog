---
title: "Fedora Linux"
date: 2021-07-07
categories:
- linux
tags:
- setups
draft: false

---


Hello guys, today I am going to bring you through a Linux distribution that I am
daily-driving.  

<!--more-->

## Fedora

I am currently using Fedora 34, a distribution of Linux associated with the
popular distribution Red-Hat Linux, usually referred to as Red-Hat Enterprise
Linux (RHEL). Though many think that Fedora is a fork of RHEL, Fedora is
actually the upstream of RHEL and where Red Hat developers test new cutting edge
Linux features. 

## I used Arch btw

To understand why I chose Fedora, I thought it would be helpful to retrace just
a little history of how my usage of Linux has changed over time. Before I used
Fedora, I was using Arch Linux ("I use Arch btw"). Like all Arch installs, you
start with almost nothing. But as time passes, I added in new features and
arrived at a system that I found to be satisfactory. 

The system had some core features that I introduced deliberately: 
- it uses the BTRFS filesystem for its snapshotting abilities. Why BTRFS?  BTRFS
  is a modern filesystem that simply blows any other filesystem (except ZFS, ZFS
  is the filesystem that inspired BTRFS) out of the water with modern feature
  sets like transparent compression using ZSTD, partitionless management of your
  disk using subvolumes, the ability to send subvolumes to another disk
  *incrementally*, and finally, checksums that ensure your files do not suffer
  from silent corruption. BTRFS makes it easy to create snapshots and backups,
  and also very easy to also restore the system. 
- ZRAM. Due to great advancements in compression technology, a new way of
  managing swap is Zram. Usually, a portion of your SSD is set aside to be used
  as a cache for you RAM. This is called the swap. Swap spaces are useful for
  the system to off-load non-frequently used data so that the RAM is ready to
  take on new data. However, recent discussions have pointed out that systems
  have come to rely on swaps so much to compensate for the lack of RAM that it
  might potentially reduce the lifespan of the SSD simply because it is writing
  so much data to the disk. Zram solves this issue by caching non-frequently
  used data on the RAM itself. That makes no sense, except when paired with good
  compression (ZSTD), it allows for a 2:1 compression of data, making room to
  cache what is traditionally swap data. RAM is always faster than swap, making
  for a more responsive system in high RAM usage scenarios, while maintaining
  disk longevity.
- Wayland is the default. Is wayland [ready](https://arewewaylandyet.com/) yet?
  I used to think wayland was a mess and always 5 years too early. But wayland
  programs do exist in the ecosystem now, and they work well! With the exception
  of some electron apps, most applications in my system use wayland. 
- [Sway](https://swaywm.org/) window manager. The Sway window manager is one of
  many environments you can use in Linux, though it is a very special one that
  has just the right set of features. It is a tiling-window manager like i3, but
  is built from the ground up to utilize wayland properly. The main pros of
  using Sway is its compositor, [wlroots](https://github.com/swaywm/wlroots). It
  is a wayland compositor that makes the experience just feels... right.
  Everything feels fast, scrolling feels smooth. It feels polished, unlike many
  other environments that support both wayland and xorg. If you want to use
  wayland, use Sway. 
- zsh. zsh is an alternative shell to the pre-installed bash. zsh was chosen
  because of the ability to introduce plugins using
  [oh-my-zsh](https://ohmyz.sh/). Things like syntax highlighting and
  auto-suggestions are already reasons enough to use zsh. 

However, since Arch is a rolling-release distribution, you have to update the
system very frequently (once every few days), and each update takes up quite a
lot of data, which is bad news if you were to ever end up in areas with slow
internet. 

I felt that I needed a system that requires less frequent updates, but maintains
all the forward looking features that I have introduced in Arch, and the answer
was Fedora. It comes with many of the aforementioned features by default, but
those that do not come out of the box can be installed without needing to use
third-party repositories. I have been using Fedora for many months now, and it
is a joy to use. Would I go back to Arch? If I have a secondary machine, someday
perhaps. But for now, Fedora is sufficient in scratching that itch that Arch
used to. 
