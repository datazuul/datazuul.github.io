---
layout: post
title: Boot a physical Windows partition in VirtualBox under Linux
author: Ralf Eichinger
toc: true
---

This post describes how to install a VirtualBox under Linux from which you can use an existing Windows installation on another partition.

# Specification

* Homepage: <https://www.virtualbox.org/>
* Current Version: 7.0.6
* Download: <https://www.virtualbox.org/wiki/Linux_Downloads>

# Installation

## Ubuntu 22.10

```sh
$ sudo apt install virtualbox
...
$ virtualbox
Hilfe - Über VirtualBox... - 6.1
```

It turns out, that manual is version 7 specific, so we download and install VirtualBox 7.

```sh
$ sudo apt remove virtualbox
```

Install downloaded package:

```sh
$ sudo apt --fix-broken install ~/Downloads/virtualbox-7.0_7.0.6-155176~Ubuntu~jammy_amd64.deb
```

Add your user to necessary groups:

```sh
$ sudo usermod -a -G disk $USER
$ sudo usermod -a -G vboxusers $USER
```

# Configuration

See manual <https://www.virtualbox.org/manual/ch09.html#rawdisk>: "Access to Individual Physical Hard Disk Partitions"

Create VMDK file:

```sh
$ fdisk -l
...
Gerät              Anfang       Ende   Sektoren  Größe Typ
/dev/nvme0n1p1       2048     206847     204800   100M EFI-System
/dev/nvme0n1p2     206848     239615      32768    16M Microsoft reserviert
/dev/nvme0n1p3     239616  999084031  998844416 476,3G Microsoft Basisdaten
/dev/nvme0n1p4  999084032 1000202239    1118208   546M Windows-Wiederherstellung
/dev/nvme0n1p5 1000202240 2000408575 1000206336 476,9G Linux-Dateisystem

$ VBoxManage list hostdrives
Drive:       /dev/nvme0n1
Model:       "SAMSUNG MZVL21T0HCLR-00BL7"
Further disk and partitioning information is not available for drive "/dev/nvme0n1". (E_ACCESSDENIED)

$ sudo VBoxManage createmedium disk --filename=windows10.vmdk --format=VMDK --variant RawDisk --property RawDrive=/dev/nvme0n1 --property Partitions=3
0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
Medium created. UUID: 0e2fa7b0-3033-4335-957c-23dfe235c350
$ ls -al windows10*.vmdk 
-rw------- 1 root root 1048576 Feb  6 11:03 windows10-pt.vmdk
-rw------- 1 root root     765 Feb  6 11:03 windows10.vmdk
$ sudo chmod 777 windows*.vmdk
```

Start VirtualBox, create a new VM without harddisk.
Add vmdk file to controller sata as harddisk..
Start VM.

Sadly it ends up with the grub shell... Stop VM.

See <https://stackoverflow.com/questions/51101487/booting-raw-disk-windows-10-vm-in-virtualbox-boots-to-grub-shell>

So we have to edit grub.cfg

```sh
$ sudo su -
# nano /boot/efi/EFI/ubuntu/grub.cfg
search.fs_uuid c16ad6af-6642-43d2-8360-a9b8227023fd root
set prefix=($root)'/boot/grub'
if [ -f $prefix/grub.cfg ]
then
    configfile $prefix/grub.cfg
else
    insmod chain
    set root=(hd0,gpt1)
    chainloader /EFI/Microsoft/Boot/bootmgfw.efi
    boot
fi
# update-grub
```

Start VM, Windows boots.

## Settings

Activate clipboard and drag and drop:

* Geräte - Gemeinsame Zwischenablage - bidirektional
* Geräte - Drag and Drop - bidirektional

Add shared folder:

* Geräte - Gemeinsame Ordner - Gemeinsame Ordner...
* Add: Ordner-Pfad "/home/ralf", Ordner-Name "ralf", "Automatisch einbinden"
* Add: Ordner-Pfad "/", Ordner-Name "Linux", "Automatisch einbinden"

Install "Gasterweiterungen":

* Geräte - Gasterweiterungen einlegen...

Run "VBoxWindowsAdditions" from new windows drive containing mounted guest additions.
Restart Windows.

Now you should see shared Folder.

In Windows: change screen resolution (e.g. to 1400 x 1050)
Change "Anzeige - Vollbildmodus"

Strg Rechts double click to leave fullscreen.
