---
layout: post
title: GitHub.com Basics
author: Ralf Eichinger
toc: true
---

UPDATES:

* 2025-03-18: Add HTTPS-authentication and GPG-signing

# Specification

Homepage: <http://www.github.com/>

# Authentication

Reference: <https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github>

GitHub-Repositories can be used with protocols SSH (e.g. `git@github.com:datazuul/gnd-rdf-xml-javalib.git`) or HTTPS (e.g. `https://github.com/datazuul/gnd-rdf-xml-javalib.git`).
Each of them needs an authentication.

## SSH

Reference: <https://docs.github.com/en/authentication/connecting-to-github-with-ssh>

For SSH authentication you have to generate a public private SSH key pair.

see <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent>

```sh
$ ssh-keygen -t ed25519 -C "ralf.eichinger@gmail.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/ralf/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase): 
Enter same passphrase again:
...
$ eval "$(ssh-agent -s)"
Agent pid 9573
$ ssh-add ~/.ssh/id_ed25519
Identity added: /home/ralf/.ssh/id_ed25519 (ralf.eichinger@gmail.com)
$ cat ~/.ssh/id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJQFMZLZ1aroMIWoic8oK06iace9wXX7tWZE5GvO41Oh ralf.eichinger@gmail.com
```

Then login to GitHub.com and add the public key to known SSH keys in your profile settings under <https://github.com/settings/keys>.

Now you should be able to connect to GitHub-repository using SSH protocol.

## HTTPS

Reference: <https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github#https>

"If you authenticate without GitHub CLI, you must authenticate with a personal access token. When Git prompts you for your password, enter your personal access token."

For HTTPS authentication you have to use a personal access token.
Personal access tokens can be generated and managed under <https://github.com/settings> - Deveoper settings - Personal access tokens (<https://github.com/settings/tokens>).
If you do not have one already, create one and store the token in a secure place.

Now you can clone your private repositories over HTTPS, e.g.

```sh
$ git clone https://github.com/your_username/myprivate-repo.git
Cloning into 'myprivate-repo'...
Username for 'https://github.com': your_username
Password for 'https://your_username@github.com': your_personal_access_token
```

entering your personal access token as password (becuase "Password-based authentication for Git has been removed in favor of more secure authentication methods.")

# Create a repository on github

* Log in
* Open <https://github.com/datazuul?tab=repositories>
* Click "New"-Button
  * fill repository name (= directory name)
  * do NOT check any checkboxes if you have a local repository prepared
* Click "Create repository"

# Add local code to a github repository

After doing this you can push your first changes (do this on command line, not in netbeans):

```sh
$ cd gnd-rdf-javalib/
$ git init
Hinweis: Als Name für den initialen Branch wurde 'master' benutzt. Dieser
Hinweis: Standard-Branchname kann sich ändern. Um den Namen des initialen Branches
Hinweis: zu konfigurieren, der in allen neuen Repositories verwendet werden soll und
Hinweis: um diese Warnung zu unterdrücken, führen Sie aus:
Hinweis: 
Hinweis:        git config --global init.defaultBranch <Name>
Hinweis: 
Hinweis: Häufig gewählte Namen statt 'master' sind 'main', 'trunk' und
Hinweis: 'development'. Der gerade erstellte Branch kann mit diesem Befehl
Hinweis: umbenannt werden:
Hinweis: 
Hinweis:        git branch -m <Name>
$ git branch -m main
$ git status
$ git add README.md pom.xml src/
$ git commit -m "first commit"
$ git remote add origin git@github.com:datazuul/gnd-rdf-xml-javalib.git
$ git push -u origin main
```

After first command line usage you can further use git client in your IDE.

## Signing Commits

References:

* [About commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification)
* [Checking for existing GPG keys](https://docs.github.com/en/authentication/managing-commit-signature-verification/checking-for-existing-gpg-keys)
* [Generating a new GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
* [Adding a GPG key to your GitHub account](https://docs.github.com/de/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account)
* [Adding a GPG key](https://docs.github.com/de/authentication/managing-commit-signature-verification/adding-a-gpg-key-to-your-github-account#adding-a-gpg-key)
* [Telling Git about your signing key](https://docs.github.com/en/authentication/managing-commit-signature-verification/telling-git-about-your-signing-key)

For signing your commits, you need a GPG key.

Use the `gpg --list-secret-keys --keyid-format=long` command to list the long form of the GPG keys for which you have both a public and private key.
A private key is required for signing commits or tags.

If you don't have a GPG key, create one:

Check GPG version:

```sh
> gpg --version
gpg (GnuPG) 2.4.7
libgcrypt 1.11.0
Copyright (C) 2024 g10 Code GmbH
License GNU GPL-3.0-or-later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: C:\Users\eichira\AppData\Roaming\gnupg
Unterstützte Verfahren:
Öff. Schlüssel: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Verschlü.: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
           CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Komprimierung: nicht komprimiert, ZIP, ZLIB, BZIP2
```

Generate key (always using defaults):

```sh
> gpg --full-generate-key
gpg (GnuPG) 2.4.7; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Bitte wählen Sie, welche Art von Schlüssel Sie möchten:
   (1) RSA und RSA
   (2) DSA und Elgamal
   (3) DSA (nur signieren)
   (4) RSA (nur signieren)
   (9) ECC (signieren, verschlüsseln) *standard*
  (10) ECC (nur signieren)
  (14) Vorhandener Schlüssel auf der Karte
Ihre Auswahl?
Bitte wählen Sie, welche elliptische Kurve Sie möchten:
   (1) Curve 25519 *standard*
   (4) NIST P-384
   (6) Brainpool P-256
Ihre Auswahl?
Bitte wählen Sie, wie lange der Schlüssel gültig bleiben soll.
         0 = Schlüssel verfällt nie
      <n>  = Schlüssel verfällt nach n Tagen
      <n>w = Schlüssel verfällt nach n Wochen
      <n>m = Schlüssel verfällt nach n Monaten
      <n>y = Schlüssel verfällt nach n Jahren
Wie lange bleibt der Schlüssel gültig? (0)
Schlüssel verfällt nie
Ist dies richtig? (j/N) j

GnuPG erstellt eine User-ID, um Ihren Schlüssel identifizierbar zu machen.

Ihr Name ("Vorname Nachname"): Your Name
Email-Adresse: your_email@foo.com
Kommentar:
Sie haben diese User-ID gewählt:
    "Your Name <your_email@foo.com>"

Ändern: (N)ame, (K)ommentar, (E)-Mail oder (F)ertig/(A)bbrechen? F
Wir müssen eine ganze Menge Zufallswerte erzeugen.  Sie können dies
unterstützen, indem Sie z.B. in einem anderen Fenster/Konsole irgendetwas
tippen, die Maus verwenden oder irgendwelche anderen Programme benutzen.
Wir müssen eine ganze Menge Zufallswerte erzeugen.  Sie können dies
unterstützen, indem Sie z.B. in einem anderen Fenster/Konsole irgendetwas
tippen, die Maus verwenden oder irgendwelche anderen Programme benutzen.
gpg: Verzeichnis `C:\\Users\\xxxxxx\\AppData\\Roaming\\gnupg\\openpgp-revocs.d' erzeugt
gpg: Widerrufzertifikat wurde als 'C:\\Users\\xxxxxx\\AppData\\Roaming\\gnupg\\openpgp-revocs.d\\FB2E1749DD3F403CEA5E9B12554F14452E01EB7E.rev' gespeichert.
Öffentlichen und geheimen Schlüssel erzeugt und signiert.

pub   ed25519 2025-03-18 [SC]
      FB2E1749DD3F403CEA5E9B12554F14452E01EB7E
uid                      Your Name <your_email@foo.com>
sub   cv25519 2025-03-18 [E]
```

The ID for this key is `FB2E1749DD3F403CEA5E9B12554F14452E01EB7E`.

Print the public key block:

```sh
> gpg --armor --export FB2E1749DD3F403CEA5E9B12554F14452E01EB7E
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEZ9lCwxYJKwYBBAHaRw8BAQdAYUhdclLHc5Q1M84P+bwBXrqIEpiZs/qHrg0j
vzuxm8e0KVJhbGYgRWljaGluZ2VyIDxyYWxmLmVpY2hpbmdlckBnbWFpbC5jb20+
iJMEExYKADsWIQT7LhdJ3T9APOpemxJVTxRFLgHrfgUCZ9lCwwIbAwULCQgHAgIi
AgYVCgkICwIEFgIDAQIeBwIXgAAKCRBVTxRFLgHrfvbyAP9JuxF8CNr4mfhMuwdK
MEIyyDynsDIxWWX/dr22TM/cbAEAxhEhSIkkpUll44NotIyb467ZslEyq45jegVj
TKfndQy4OARn2ULDEgorBgEEAZdVAQUBAQdAnZFb6OahhkXvRnIfgp1+aoIsVAiT
5H4I/7ZOy/4Rt0QDAQgHiHgEGBYKACAWIQT7LhdJ3T9APOpemxJVTxRFLgHrfgUC
Z9lCwwIbDAAKCRBVTxRFLgHrfmUwAP49HUbX73Fll4pmrqDlLrKabjK07a65WgP8
/lyF89GlsAD/dF0WYaHkZiufebp1vVYIYMWyy7p2gDREK2H4IDZjRwU=
=PbgE
-----END PGP PUBLIC KEY BLOCK-----
```

Add this key to your GitHub-GPG-Keys. Copy the whole output from above `-----BEGIN ... -----END PGP PUBLIC KEY BLOCK-----` (including the comment lines)
and add it under <https://github.com/settings/keys> section "GPG keys".

After this, tell GIT about your signing key:

```sh
> git config --global --unset gpg.format
> gpg --list-secret-keys --keyid-format=long
...
> git config --global user.signingkey 3AA5C3AS4371567BD2
```

Optionally, to configure Git to sign all commits and tags by default, enter the following command:

```sh
git config --global commit.gpgsign true
git config --global tag.gpgSign true
```

Or you can configure it in your `.gitconfig` using 

```sh
[user]
	name = Your Name
	email = ****@****
	signingkey = 3AA5C3AS4371567BD2
```
