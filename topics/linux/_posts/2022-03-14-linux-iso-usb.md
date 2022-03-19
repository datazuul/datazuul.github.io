---
layout: post
title: Create a bootable Linux installation USB device
author: Ralf Eichinger
toc: true
---

For installation of a linux system you need to create a bootable installation medium.
In former times this was often a bootable DVD or CD.
Nowadays it is usually an USB stick.

This post describes how to create such a bootable USB stick.

1. Download ISO:

    First of all download an installation ISO file from your favorite Linux distribution.\
    Example: http://ftp.uni-kl.de/pub/linux/ubuntu-dvd/xubuntu/releases/21.10/release/xubuntu-21.10-desktop-amd64.iso

2. Get device name of USB stick

    Plugin your USB stick and list devices:

    ```sh
    $ lsblk
    NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sda           8:0    1  29,5G  0 disk 
    ├─sda1        8:1    1   2,5G  0 part 
    ├─sda2        8:2    1   2,7M  0 part 
    └─sda3        8:3    1    27G  0 part 
    nvme0n1     259:0    0 953,9G  0 disk 
    ├─nvme0n1p1 259:1    0   260M  0 part /boot/efi
    ├─nvme0n1p2 259:2    0    16M  0 part 
    ├─nvme0n1p3 259:3    0  73,7G  0 part 
    ├─nvme0n1p4 259:4    0  1000M  0 part 
    ├─nvme0n1p5 259:5    0 839,2G  0 part /
    └─nvme0n1p6 259:6    0  39,7G  0 part [SWAP]
    $
    ```

    By the size of the example stick (32 GB) `/dev/sda` turns out to be the target device.

3. Write downloaded ISO (if = input file) to USB device (of = output file)

```sh
$ sudo dd bs=4M if=/home/ralf/Downloads/xubuntu-21.10-desktop-amd64.iso of=/dev/sda conv=fdatasync status=progress
500+1 Datensätze ein
500+1 Datensätze aus
2097561600 Bytes (2,1 GB, 2,0 GiB) kopiert, 94,8306 s, 22,1 MB/s
$
```

That's it.

Before booting from USB device you have to make sure the device is first in boot sequence in BIOS.