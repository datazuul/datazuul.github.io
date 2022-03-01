---
layout: post
title: Installation Debian 3.0r0 ("Woody") auf Midi-Tower IBM Aptiva
author: Ralf Eichinger
toc: true
---

# Spezifikation der Hardware

<table>
<tr>
<td>Prozessor</td>
<td>Intel Pentium II (400 MHz)</td>
</tr>

<tr>
<td>Hauptspeicher</td>
<td>192 MB</td>
</tr>

<tr>
<td>Festplatte(n)</td>
<td markdown="1">
  6 GB IDE (WDC AC26400B) ATA,\
540 MB IDE (Conner CFS541A) ATA
</td>
</tr>

<tr>
<td>Bildschirm</td>
<td>17“ "Modular"</td>
</tr>

<tr>
<td>Grafikkarte (onBoard)</td>
<td>NVidia Riva 128</td>
</tr>

<tr>
<td>Soundkarte (onBoard)</td>
<td>SoundBlaster AudioPCI 64V (ES1373)</td>
</tr>

<tr>
<td>Netzwerkkarte</td>
<td>Realtek RTL-8029AS (PCI)</td>
</tr>

<tr>
<td>USB (onBoard)</td>
<td>2 x</td>
</tr>

<tr>
<td>USB 2.0 (interne Karte)</td>
<td>Adaptec AUA2000/3100LP (PCI)</td>
</tr>

<tr>
<td>Disketten-Laufwerk</td>
<td>3,5“</td>
</tr>

<tr>
<td>CD-Laufwerk</td>
<td>8x DVD-ROM Laufwerk (Trust)</td>
</tr>
</table>

# Installation

## Booten von CD

Legen Sie die CD ein und starten Sie den Rechner.

## Begrüssungsbildschirm

Nach kurzer Zeit erscheint Debian mit einer Begrüssungsseite

```
                  Welcome to Debian GNU/Linux 3.0!
...
Press <F1> for help, or <ENTER> to boot.

boot:
```

Drücken Sie einfach auf `<ENTER>`.

## Sprachauswahl

```
                  Choose The Language
...
`de-` Dies auswählen und Return drücken, um auf Deutsch fortzufahren
...
```

```
                  Wählen Sie den Sprachraum

Deutsch (Deutschland)  
...
```

## Versions-Info

```
                          Versions-Info  
                  Software in the Public Interest  
                           präsentiert  
                   *** Debian GNU/Linux 3.0 ***

Dies ist Debians Installationssystem, ..., in der Version 3.0.23.
...
```

## Hauptmenü

Sie befinden sich anschließend im Hauptmenü des Installationssystems, das Sie durch alle Schritte der Installation führt. Als erster Schritt wird der Punkt "Tastaturkonfiguration" aufgeführt.

## Tastaturkonfiguration

```
                  Wählen Sie den Typ der Tastatur aus.
...
qwertz/de-latin1-nodeadkeys: Deutsch
```

## Partitionierung einer Festplatte

```
                  Festplatte auswählen

/dev/hda
...
```

### Einschränkungen von LILO

```
                  Einschränkungen von LILO

LILO, der Boot-LOader für Linux, und das alternativ installierbare
'mbr'-Programm unterstützen die Verwendung großer Festplatten, wenn sich
in dem Rechner ein modernes BIOS (neuer als 1194-98) befindet, welches
LBA und die "Enhanced Disk Drive Support Specification" unterstützt.
Wenn Sie sicher sind, dass Ihr BIOS die INT 0x13-Erweiterung für
große Festplatten unterstützt, können Sie die Festplatte so
partitionieren, wie Sie mögen.
```

### cfdisk 2.11n

```
                                   cfdisk 2.11n

                               Disk Drive: /dev/hda
                              Size: 6448619520 bytes
                  Heads: 255  Sectors per Track: 63  Cylinders: 784

Name            Flags   Part Type       FS Type         [Label] Size (MB)
hda1                    Primary         Linux swap               403.04
hda2             Boot   Primary         Linux                     32.91
hda3                    Primary         Linux                   3002.23
hda4                    Primary         Linux                   3010.46

[ Write ] - [ Quit ]
```

## Formatieren und Einbinden einer Swap-Partition

```
                              Swappartition auswählen
...
/dev/hda1: Linux swap
...
```

```
                     Suche nach fehlerhaften Blöcken durchführen?
...
Soll die Suche nach fehlerhaften Blöcken auf /dev/hda1
durchgeführt werden?
                                   ... <Nein>
```

```
                                 Sind Sie sicher?
...
Möchten Sie die Partition wirklich formatieren?
                                   <Ja> ...
```

## Formatieren und Einbinden einer Linux-Partition

Die 4 Partitionen müssen nun formatiert und eingebunden werden. Als Root-Dateisystem ("/") ist die Partition /dev/hda3 vorgesehen.

```
                        Wählen Sie eine Partition aus

Wählen Sie die Partition aus, die mit dem Linux'
"Ext2"-Dateisystem formatiert werden soll.
                           /dev/hda3: Linux native
                                   ...
```

```
                 Suche nach fehlerhaften Blöcken durchführen?
...
Soll die Suche nach fehlerhaften Blöcken auf /dev/hda3
durchgeführt werden?
                               ... <Nein>
```

```
                          Sind Sie sicher?
...
Möchten Sie die Partition wirklich formatieren?
                             <Ja> ...
```

```
                Dies als das Wurzeldateisystem mounten?

Sie müssen das Wurzeldateisystem ("/") einbinden, bevor Sie
andere Dateisysteme einbinden können. Möchten Sie /dev/hda3
als Root-Dateisystem einbinden?
                             <Ja> ...
```

Die anderen Partitionen (bis auf die zweite Festplatte /dev/hdd) wurden nacheinander über den Hauptmenü-Punkt "Formatieren und Einbinden einer Linux-Partition" folgendermaßen formatiert und eingebunden:

```
/dev/hda2    /boot: Eine kleine (5-10MB) Partition am Anfang der Platte.
/dev/hda4    /home: Heimatverzeichnisse der Benutzer
```

## Installation von Kernel und Modulen

```
                      Eine Debian-CD-ROM gefunden
...
                               <Ja> ...
```

## Laden und Konfigurieren der Treibermodule

```
                    Anmerkung zu geladenen Treibern
...
                              <Weiter>
```

```
                        Kategorie auswählen
...
Bitte wählen Sie die Kategorie der Module.

      Beenden  Alle Module erledigt. Zurück zum vorherigen Menü
      block    Platten und plattenartige Geräte.
      net      Netzwerkkarten und Netzwerkprotokolle.
      fs       Treiber, um auf verschiedene Dateisysteme zugreifen zu können
      cdrom    Gerätetreiber für CD-ROM Laufwerke.
      misc     Treiber, die in keine der andern Kategorien passen.
```

Als Module wurden gewählt:

```
net  pci-scan ...
```

## Konfiguration des Netzwerks

```
                      Wählen Sie den Rechnernamen
...
aptiva
                                <OK> ...

                Automatische Konfiguration des Netzwerks
...
Soll diese Schnittstelle automatisch über BOOTP oder DHCP
konfiguriert werden? ...
                              ... <Nein>

                      Geben Sie die IP-Adresse an.
...
192.168.1.21

                  Bitte geben Sie die Netzwerkmaske an.
...
255.255.255.0

              Wie lautet die IP-Adresse des Gateway-Systems?
...
192.168.1.1

                Geben Sie den Namen Ihrer (IP-) Domäne an.
...
intranet

                Geben Sie die IP-Adressen der DNS-Server an
...
(leer)
```

## Installation des Basissystems

```
                  Wählen Sie das Installationsmedium aus
...
CDROM: CD-ROM Laufwerk
...

                    Bitte legen Sie die CD-ROM ein
...
                              <Weiter>

                            Bitte warten
...

                    Wählen Sie den Archiv-Pfad aus.
...
                              /instmnt

              Installiere das Basissystem, bitte warten...
...
```

## Das System bootfähig machen.

```
                Wohin soll der Boot-Loader LILO installiert werden?
...
/dev/hda: Installation von LILo in den MBR (im Zweifelsfall).
...

                            LILO absichern
...
```

## Neustart des Systems

```
                    Neustart des Systems
...

                    Neustart des Systems?
...
Soll das System neugestartet werden?
                          <Ja> ...
```

Nehmen Sie beim Bootvorgang die CD aus dem Laufwerk.

# Konfiguration

Nach dem Booten führt Sie Debian durch die weitere Konfiguration des Systems

```
                  Debian System Configuration
...
If you want to revisit this setup process at a later date, just run
/usr/bin/base-config.
                           <Ok>
```

## Zeitzone

```
                    Time Zone Configuration
...
The hardware clock says the time is now Tue Sep 24 19:27:45 2002.
Is the hardware clock set to GMT?
                          ... <No>
```

In diesem Fall haben wir mit "No" geantwortet, da die Zeit die lokale Ortszeit wiedergab und nicht die GMT (Greenwich Mean Time).

```
                    Time Zone Configuration

Pick the geographic area in which you live.
What area do you live in?

    ...
    Europe
    ...
                          <Ok> ...

...
Select a city or time zone:

    ...
    Berlin
    ...
                          <OK> ...
```

## Passwort

```
                    Password setup

Md5 passwords are more secure and allow for passwords longer than 8
characters to be used. However, they can cause compatibility problems
if you are using NIS or sharing password files with older systems.

Shall I enable md5 passwords?
                       ... <No>
```

```
                    Password setup

Shadow passwords make your system more secure because nobody is able to
view even encrypted passwords. Passwords are stored in a separate file
that can only be read by special programs. We recommend the use of
shadow passwords. However, if you're going to use NIS you could run
into trouble.

Shall I enable shadow passwords?
                        <Yes> ...
```

```
                    Password setup

...
Note that you will not be able to see the password as you type it.

Enter a password for root:

                        <Ok> ...

Please enter the same root password again to verify you have typed it
correctly.

Re-enter password to verify:  

                        <Ok> ...
```

```
                    Password setup
...
Shall I create a normal user account now?
                        <Yes> ...
...
Enter a username for your account:
ralf
                        <Ok> ...
...
Enter the full name of the new user. Your full name is a good choice.

Enter a full name for the new user:
Ralf Eichinger
                        <Ok> ...

A good password will contain a mixture of letters, numbers and
punctuation and will be changed at regular intervals.

Enter a password for the new user:

                        <Ok> ...

Please enter the same user password again to verify you have typed it
correctly.

Re-enter password to verify:  

                        <Ok> ...
```

## PCMCIA

```
                    Debian System Configuration

It seems your system doesn't need PCMCIA. It was installed with the
rest of the kernel, but can be removed now.

Shall I remove the pcmcia packages?
                            <Yes> ...
```

## PPP-Verbindung

```
                    Debian System Configuration

If you have an account on an ISP, and you want to use it to fetch the
packages to install on the system from the Internet, you may configure
the PPP service now, and I will open a PPP connection to your ISP.

Do you want to use a PPP connection to install the system?
                            ... <No>
```

## APT-Zugriffsmethode

```
                    Apt Configuration

Apt can access the Debian archive in a variety of ways. Choose the
access method apt should use. For example if you have a Debian cd,
select "cdrom", while if you plan to install via a Debian mirror,
choose "ftp" or "http".

You probably used a CD to install the Debian base system, but it is not
currently in the drive. You should probably just insert it and select
"cdrom".

Choose the method apt should use to access the Debian archive:

    cdrom
    ...
                        <Ok> ...
```

Legen Sie die CD 1 ein und bestätigen Sie mit "Ok".

## Einlesen der Paketindizes

```
...
Scanning Disc for index files..
```

```
                    Apt Configuration

If you have another Debian CD (for example, the second in a two CD
set), you should insert and scan it now.

Scan another CD?
                        <Yes> ...
```

Legen Sie (falls vorhanden) die CD 2 ein und bestätigen Sie mit "Ok".

```
...
Scanning Disc for index files..
```

Fahren Sie solange fort, bis die Indizes aller Ihrer CDs eingelesen wurden. Fahren Sie dann fort mit:

```
                    Apt Configuration

If you have another Debian CD (for example, the second in a two CD
set), you should insert and scan it now.

Scan another CD?
                        <No> ...
```

## Hinzufügen anderer APT-Quellen

```
                    Apt Configuration

Apt is now configured, and should be able to install Debian packages.
However, you may want to add another source to apt, so it can download
packages from more than one location.

Add another apt source?
                      ... <No>
```

Wir antworten mit "No", da wir vorerst nur von CD installieren wollen.

## Sicherheits-Updates

```
                    Apt Configuration

Debian provides timely security updates via the website
http://security.debian.org/. Should these updates be tracked each time
you upgrade your system? If you are on the internet, this is generally
a good idea.

Use security updates from security.debian.org?
                        <Yes> ...
```

Zu diesem Zeitpunkt ist das System noch nicht mit dem Netzwerk (und dem Internet) verbunden. Eine Kontaktaufnahme schlägt daher vorerst fehl:

```
                    Apt Configuration

Cannot access security updates.

I cannot access the security updates on security.debian.org, so those
updates will not be made available to you at this time. You may want to
investigate this later. Commented out entries for security.debian.org
have been added to the bottom of /etc/apt/sources.list. The error
message I received from apt was:

Failed to fetch
http://security.debian.org/dists/stable/updates/main/binary-i386/Packages
Something wicked happened resolving 'security.debian.org:http' (-3)
Failed to fetch
http://security.debian.org/dists/stable/updates/main/binary-i386/Packages
Something wicked happened resolving 'security.debian.org:http' (-3)
E: Some index files failed to download, they have been ignored, or old
ones used instead.
                          <Ok>
```

## Paket-Auswahl

```
                    Debian System Configuration

At this point, your Debian system is very minimal, composed of just
enough software to bootstrap a complete system. ... Tasksel lets you
pick from various predefined collections of software ...

Run tasksel?
                            <Yes> ...
```

```
Debian Task Installer v1.18

                      Select tasks to install

-- End-user --
x        X window system
x        desktop environment
         dialup system
x        games
x        Debian Jr.
         laptop system
x        scientific applications
-- Servers --
         SQL database
         DNS server
         file server
         mail server
         usenet news server
         print server
         conventional unix server
         web server
-- Development --
x        C and C++
         fortran
x        Python
x        Tcl/Tk
-- Localization --
         simplified Chhinese environment
         traditional Chinese environment
         Cyrillic environment
         French environment
x        German environment
         Japanese environment
         Korean environment
         Polish environment
         Russian environment
         Spanish environment
-- Miscellaneous --
x        TeX/LaTeX environment

                  <Finish> ... ...
```

## Paket-Installation

```
...
0 packages upgraded, 492 newly installed, 0 to remove and 0  not upgraded.
Need to get 0B/322MB of archives. After unpacking 910 MB will be used.
Do you want to continue? [Y/n] Y
Media Change: Please insert the disc labeled 'Debian GNU/Linux 3.0 r0 _Woody_ -
Official i386 Binary-1 (20020718)' in the drive '/cdrom' and press enter
```

## Lokalisierung

```
                    Configuring Locales

You can choose locales to be generated by selecting locales you want.
Selected locales will be saved to '/etc/locale.gen' file. You can also
manually edit this file. You need to run 'loocale-gen' after edit the
file.

Select locales to be generated.
    ...
    x     de_DE ISO-8859-1
    ...
    x     de_DE@euro ISO-8859-15
...
Which locale should be the default in  the system environment?
    ...
    de_DE@euro
                          <Ok>
```

## Drucker

```
                    Configuring Psfontmgr
Those who have a PostScript printer should answer Yes, and register
fonts installed in the printer to have PostScript-outputting
applications know what PostScript fonts are available, by using
defoma-psfont-installer.

Do you have a PostScript Printer?
                          ... <No>
```

```
                  Paper Size Configuration
...
Which papersize should be the default?

    a4
    ...
                          <Ok>
```

## CVS

```
                    Configuring Cvs
List the directories that are the roots of your repositories, separated
by colons.

These repositories can be exported by the pserver, have their history
files rotated automatically every week, and general repository security
checks will be performed on them.

If you wish to create a new repository, enter the path where you wish
to create it. You will then be given the option of creating it later.

Where are your repositories?
/var/lib/cvs
                          <Ok>
...

Do you wish to fix invalid repositories?

      create
      ...
                          <Ok>
...
Should the CVS pserver be enabled?
                        ... <No>
```

## GDM

```
                    Configuring Gdm
A display manager is a program that provides graphical login
capabilities for the X Window System.
...

Select the desired default display manager.

      gdm
      ...
                        <Ok>
```

## Gnuplot

```
                    Configuring Gnuplot
In order to enable ordinary users to use SVGA console graphics gnuplot
needs to be set up as setuid root. Please note that this is usually
considered to be a security hazard.

Do you want to install gnuplot setuid root?
                        ... <No>
```

## Mozilla-Browser

```
                    Configuring Mozilla-Browser
...
Do you want to use FreeType2 support on Mozilla?
                          <Yes> ...

...This setting
will be saved into /etc/mozilla/mozillarc and can be overriden with
your ~/.mozillarc.

Please choose your sound daemon's dsp wrapper.

      ...
      auto
      ...
                          <Ok>
```

## X-Windows

```
                    Configuring Xserver-common
The /etc/X11/Xwrapper.config file, which contains configuration
information controlling who is permitted to start the X server and how
it is invoked, can be habdled automatically by debconf, or manually by
you.

Note that only specific, marked sections of the configuration file will
be handled by debconf if you select this option; if those markers are
absent, you will have to update the file manually, or move or delete
the file.

Manage X server wrapper configuration file with debconf?
                            <Yes> ...
```

```
                    Configuring Xserver-xfree86
The /etc/X11/XF86Config-4 file, which contains the configuration
information for the XFree86 version 4 X server, can be handled
automatically by debconf, or manually by you.

Note that only specific, marked sections of the configuration file will
be handled by debconf if you select this option; if those markers are
absent, you will have to update the file manually, or move or delete
the file.

Manage XFree86 4.x configuration file with debconf?
                            <Yes> ...
```

```
                    Configuring Xserver-xfree86
For the X Window System graphical user interface to operate correctly,
it is necessary to select a video card driver for the X server.
Drivers are typically named for the video card or chipset manufacturer,
or for a specific model or family of chipsets.

Select the desired X server driver.

      ...
      nv
      ...
                              <Ok>
```

```
                    Configuring Xserver-xfree86
Rather than communicating directly with the video hardware, the X
server may be configured to perform some operations, such as video mode
switching, via the kernel's framebuffer driver.

In theory, either approach should work, but in practice, sometimes one
does and the other does not. Enabling this option is the safe bet, but
feel free to turn it off if it appears to cause problems.
Use kernel framebuffer device interface?
                            ... <No>
```

## Tastatur

```
                    Configuring Xserver-xfree86
For the X server to handle your keyboard correctly, an XKB rule set
must be chosen.

Users of most keyboards should enter "xfree86". Users of Sun Type 4
and Type 5 keyboards, however, should enter "sun".

Advanced users can use any defined XKB rule set. See the
/etc/X11/xkb/rules directory for available rule sets.

If you don't know what rule set to use, enter "xfree86".

Please select the XKB rule set to use.

      xfree86
                              <Ok>
```

```
                    Configuring Xserver-xfree86
Please select your keyboard model.

      pc104
                              <Ok>
...

Please select your keyboard layout.

      de
                              <Ok>
...
Many keyboard layouts support an option to treat "dead" keys such as
non-spacing accent marks and diaereses as normal spacing keys, and if
this is the preferred behavior, enter "nodeadkeys".

Advanced users can use any variant supported by the selected XKB
layout. See the /etc/X11/xkb/symbols directory for the file
corresponding to your selected layout for available variants.
...
Please select your keyboard variant.

      nodeadkeys
                              <Ok>
```

```
                    Configuring Xserver-xfree86
For the X server to handle your keyboard as you desire, keyboard
options may be entered. Available options depend on which XKN rule set
was previously selected. Not all options will work with every keyboard
model and layout.

For example, if you wish the Caps Lock key to behave as an additional
Control key, you may enter "ctrl:nocaps"; if you would like to switch
the Caps Lock and left Control keys, you may enter "ctrl:swapcaps".

Advanced users can use any options compatible with the selected XKB
model, layout and variant. See the /etc/X11/xkb/symbols directory
available options files, which define only partial keyboard
translations. (For example, /etc/X11/xkb/symbols/ctrl.)

If you don't know what options to use, leave this entry blank.

Please select your keyboard options.  

                                <Ok>
```

## Maus

```
                    Configuring Xserver-xfree86
...
Please choose your mouse port.

      /dev/psaux
      ...
                                <Ok>

Please choose the entry that best describes your mouse.

      PS/2
      ...
                                <Ok>
```

## Monitor

```
                    Configuring Xserver-xfree86
...
Is your monitor an LCD device?
                             ... <No>
...
Please choose a method for selecting your monitor characteristics.

      ...
      Medium
                                <Ok>
...
Please select your monitor's best video mode.

      1280x960 @ 60Hz
      ...
                                <Ok>
...
Select the video modes you would like the X server to use.

      ...
      1152x864
      1024x768
      800x600
      640x480
                                <Ok>
...
Please select your desired default solor depth in bits.

      24
      ...
                                <Ok>
```

## Entpacken der Packages

Nach der Konfiguration von X-Windows beginnt das Entpacken der Packages auf der Festplatte. Dies kann sehr lange dauern... (ca. 15 min).

## Konfiguration der Packages

Gleich im Anschluß werden die Pakete konfiguriert. Teilweise werden Eingaben von Ihnen verlangt.
