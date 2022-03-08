---
layout: post
title: Apache NetBeans IDE - Basics
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://netbeans.apache.org/>
* Version: 13
* File(s): <https://dlcdn.apache.org/netbeans/netbeans-installers/13/Apache-NetBeans-13-bin-linux-x64.sh>

# Installation

Execute downloader installer:

```sh
$ cd ~/Downloads
$ /bin/sh Apache-NetBeans-13-bin-linux-x64.sh
```

# Configuration



# Troubleshooting

## Change default JDK

As it is not possible to change or remove the JDK marked with "(Default)" in Netbeans -> Tools -> Java Platform,
you have to edit `netbeans.conf`:

```sh
$ cd your_installation_directory/netbeans-13/netbeans/etc
$ nano netbeans.conf
...
##netbeans_jdkhome="/usr"
netbeans_jdkhome="/usr/lib/jvm/java-17-openjdk-amd64"
...
```

## Accent / "No dead keys" not working

It is a problem with ibus. Check current settings:

```sh
$ echo $XMODIFIERS
@im=ibus
```

Solution is to remove this setting during start of Netbeans.

Change .desktop-File that starts Netbeans:

```sh
$ nano ~/.local/share/applications/Apache\ NetBeans-13.desktop
...
Exec=env XMODIFIERS="" "/home/ralf/Applications/netbeans-13/netbeans/bin/netbeans"
...
```

## Alt-Shift-Upd/Down not working

Your desktop environment is using the key combination. Deactivate this.

elementaryOS:

Systemeinstellungen - Tastatur - Tastenbelegung - Tastaturbelegung ändern: von "Alt+Umschalttaste" ändern auf "Deaktiviert"