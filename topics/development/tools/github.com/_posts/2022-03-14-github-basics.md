---
layout: post
title: GitHub.com Basics
author: Ralf Eichinger
toc: true
---

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