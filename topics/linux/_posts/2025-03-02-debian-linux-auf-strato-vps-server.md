---
layout: post
title: Debian Linux 12 auf einem Strato VPS (Virtual Private Server)
author: Ralf Eichinger
toc: true
---

In diesem Artikel beschreiben wir die Neuinstallation und Grundeinrichtung eines VPS-Servers bei dem Hosting-Anbieter [Strato](https://www.strato.de/).

# Installation

Loggen Sie sich im Kundenbereich von [Strato](https://www.strato.de/) ein und wählen Sie Ihr VPS-Paket aus.

Im Abschnitt "Serverdetails" klicken Sie auf "Neuinstallation".

![Neuinstallations-Dialog](/assets/topics/linux/strato-vps/neuinstallation-vps-strato.png)

* Betriebssystem: Debian 12
* Passwort: Geben Sie ein sicheres Passwort für den "root"-User ein (für einen Login im Notfall via VNC Konsole
* Public SSH-Key: Geben Sie den Publickey Ihres Benutzers ein, der sich als "root" über SSH einloggen können soll. Dieser befindet sich auf einem Linux-System im Homeverzeichnis des Benutzers, z.B. in der Datei `~/.ssh/id_ed25519.pub`. Kopieren Sie den ganzen Inhalt (`ssh-ed2551 ...`) in das Eingabefeld.

Setzen Sie den Haken bei "Mir ist bekannt, dass bei einer Neuinstallation alle auf dem Server vorhandenen Daten unwiderruflich gelöscht werden. Bei einer Erstinstallation gehen selbstverständlich keine Daten verloren." und Klicken Sie für die Neuinstallation auf "Neuinstallation starten".

Nach relativ kurzer Zeit steht das neu installierte Debian 12 auf dem VPS zur Verfügung.

# Konfiguration

Loggen Sie sich auf dem Server ein:

```sh
$ ssh -l root <ip oder hostname>
```

## Benutzer mit sudo-Rechten anlegen

Es empfiehlt sich nicht mit dem `root`-User auf einem System zu arbeiten.
Legen Sie einen Benutzer an, mit dem Sie in Zukunft mit `sudo`-Rechten arbeiten wollen.

```sh
# useradd -m neuer_benutzername
```

Es wird der Benutzer `neuer_benutzername` und sein home-Verzeichnis unter `/home` angelegt.

`bash` als Standardshell setzen:

```sh
# usermod --shell /bin/bash neuer_benutzername
```

Passwort setzen für Benutzer:

```sh
# passwd neuer_benutzername
New password: 
Retype new password: 
passwd: password updated successfully
```

Den neuen Benutzer zur `sudo`-Gruppe hinzufügen:

```sh
# usermod -aG sudo neuer_benutzername
```

Test: Dateien im Homeverzeichnis des Benutzers `root` auflisten

```sh
# su - neuer_benutzername
$ sudo ls -al /root
[sudo] password for neuer_benutzername:
...
```

Wenn die Dateien aufgelistet werden können, hat der neue Benutzer mit `sudo` Root-Rechte.


## Benutzer-Login mit Public-Private-Key einrichten

Um den Login mit dem neuen Benutzernamen auf dem Server zu ermöglichen, muß der Public-Key des Benutzers des Clients (Zeile aus `cat .ssh/id_ed25519.pub`) auf dem Server als "authorized key" hinzugefügt werden:

```sh
# su - neuer_benutzername
$ mkdir -p ~/.ssh
$ echo "ssh-ed25519 ..." >> ~/.ssh/authorized_keys 
```

Test: Login vom Client aus:

```sh
$ ssh -l neuer_benutzername servername
```

## Firewall einrichten

Als nächstes sollten Sie zeitnah [eine Firewall einrichten]({% post_url topics/linux/2025-03-02-ufw-firewall-debian-linux %}
), um den Server so "dicht" wie möglich zu machen.

# Betrieb

## Betriebssystem aktuell halten

Halten Sie Ihr Betriebssystem aktuell, um Sicherheitslücken zu vermeiden:

```sh
sudo apt-get update && sudo apt-get upgrade
```
