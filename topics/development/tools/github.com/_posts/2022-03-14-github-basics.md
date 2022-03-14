---
layout: post
title: GitHub.com Basics
author: Ralf Eichinger
toc: true
---

# Specification

Homepage: <http://www.github.com/>

# Authentication

For authentication you have to generate a public private key pair.

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

Then login to GitHub.com and add the public key to known keys in your profile settings.

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

After first command line usage you can further use git client in netbeans.