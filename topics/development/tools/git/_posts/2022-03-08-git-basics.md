---
layout: post
title: GIT Basics
author: Ralf Eichinger
toc: true
---

# Specification

Homepage: <http://www.git-scm.com/>

# Installation

##  Commandline Client

Debian base:

```sh
$ sudo apt-get install git
$ git --version
git version 2.25.1
```

SuSE based:

```sh
$ sudo zypper install git-core
Refreshing service 'spacewalk'. 
Loading repository data... 
Reading installed packages... 
Resolving package dependencies... 

The following NEW packages are going to be installed: 
  git-core git-gui gitk perl-Error 

The following packages are not supported by their vendor: 
  git-core git-gui gitk perl-Error

4 new packages to install. 
Overall download size: 2.7 MiB. After the operation, additional 10.5 MiB will be used. 
Continue? [y/n/? shows all options] (y): y 
Retrieving package perl-Error-0.17015-1.13.x86_64 (1/4), 26.0 KiB (48.0 KiB unpacked) 
Retrieving package git-core-1.7.12.4-0.9.1.x86_64 (2/4), 2.3 MiB (8.9 MiB unpacked) 
Retrieving package gitk-1.7.12.4-0.9.1.x86_64 (3/4), 134.0 KiB (573.0 KiB unpacked) 
Retrieving package git-gui-1.7.12.4-0.9.1.x86_64 (4/4), 218.0 KiB (1.1 MiB unpacked) 
Installing: perl-Error-0.17015-1.13 [done] 
Installing: git-core-1.7.12.4-0.9.1 [done] 
Installing: gitk-1.7.12.4-0.9.1 [done] 
Installing: git-gui-1.7.12.4-0.9.1 [done]

$ git --version 
git version 1.7.12.4
```

## Eclipse Plugin

Eclipse - Help – Eclipse Marketplace.../Install Software ... Install:

* EGit - Git Team Provider
  * Eclipse EGit
  * Eclipse Jgit

# Configuration

The git config command lets you configure your Git installation (or an individual repository) from the command line.
This command can define everything from user info to preferences to the behavior of a repository.
Several common configuration options are listed below.

## List configuration values

After a fresh installation there is no configuration existing. To list all configured values:

System wide configuration:

```sh
$ git config --system -l
fatal: Konnte Konfigurationsdatei '/etc/gitconfig' nicht lesen.: Datei oder Verzeichnis nicht gefunden
```

Global (linux user specific) configuration:

```sh
$ git config --global -l
fatal: Konnte Konfigurationsdatei '/home/ralf/.gitconfig' nicht lesen.: Datei oder Verzeichnis nicht gefunden
```

Local (git repository specific) configuration:

```sh
$ git config --local -l
fatal: --local kann nur innerhalb eines Git-Repositories verwendet werden
```

## Define the author name

The first thing you’ll want to do after installing Git is tell it your name/email and customize some of the default settings.

Define the author name to be used for all commits in the current repository:

```sh
$ git config user.name <name>
```

Typically, you’ll want to use the `--global` flag to set configuration options for the current user.
Define the author name to be used for all commits by the current user:

```sh
$ git config --global user.name <username>
```

## Define the author email

Define the author email to be used for all commits by the current user:

```sh
$ git config --global user.email <email>
```

## Create a shortcut for a Git command

Create a shortcut for a Git command:

```sh
$ git config --global alias.<alias-name> <git-command>
```

## Define the default text editor

Define the text editor used by commands like git commit for all users on the current machine.
The `<editor>` argument should be the command that launches the desired editor (e.g., nano).

```sh
$ git config --system core.editor <editor>
```

## Typical initial configuration session

A typical initial configuration might look something like the following:

```sh
# Tell Git who you are
$ git config --global user.name "John Smith"
$ git config --global user.email john@example.com

# Select your favorite text editor
$ git config --global core.editor vim

# Add some SVN-like aliases
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.up rebase
$ git config --global alias.ci commit
```

This will produce the ~/.gitconfig file in the next section.

## Edit the global configuration file

Open the global configuration file in a text editor for manual editing.


```sh
$ git config --global --edit
```

All configuration options are stored in plaintext files, so the git config command is really just a convenient command-line interface.
Typically, you’ll only need to configure a Git installation the first time you start working on a new development machine,
and for virtually all cases, you’ll want to use the `--global` flag.

Git stores configuration options in three separate files, which lets you scope options to individual repositories, users, or the entire system:

* `<repo>`/.git/config – Repository-specific settings.
* ~/.gitconfig – User-specific settings. This is where options set with the --global flag are stored.
* $(prefix)/etc/gitconfig – System-wide settings.

When options in these files conflict, local settings override user settings, which override system-wide.
If you open any of these files, you’ll see something like the following:

```
[user] 
name = John Smith
email = john@example.com

[alias]
st = status
co = checkout
br = branch
up = rebase
ci = commit

[core]
editor = vim
```

You can manually edit these values to the exact same effect as git config.

# Usage

See <http://try.github.com>

## Initialize a repository

Git allows groups of people to work on the same documents (often code) at the same time,
and without stepping on each other's toes. It's a distributed version control system.

Our terminal prompt below is currently in an octobox directory.
To initialize a Git repository here, type the following command:

```sh
$ cd minimvc-master
$ git init
Initialized empty Git repository in /home/ralf/DEV/SOURCES/minimvc-master/.git/
$
```

**Directory**: A folder used for storing multiple files.<br>
**Repository**: A directory where Git has been initialized to start version controlling your files.

## Initialize a repository on remote server

See “Initialize a repository” and then push local repository to remote server:

```sh
$ git remote add origin git@somereposerver.de:/examples/springmvc-thymeleaf-javaconfig.git
$ git status
$ git add --all
$ git commit -m “initial import”
$ git push origin master
FATAL: W any examples/springmvc-thymeleaf-javaconfig reichinger DENIED by fallthru
(or you mis-spelled the reponame)
fatal: Could not read from remote repository.
$
```

Please make sure you have the correct access rights and the repository exists.

In gitolite-admin add permissions/project (initializes repository, too):

```sh
$ cd gitolite-admin
$ git pull
$ nano conf/gitolite.conf
  repo examples/springmvc-thymeleaf-javaconfig 
    RW+                     =   @all
$ git add --all
$ git commit -m "added examples"
$ git push
…
Counting objects: 7, done. 
Delta compression using up to 8 threads. 
Compressing objects: 100% (3/3), done. 
Writing objects: 100% (4/4), 411 bytes | 0 bytes/s, done. 
Total 4 (delta 1), reused 0 (delta 0) 
remote: WARNING: doesn't make sense to supply a ref ('refs/heads/devel') for 'R' rule 
remote: WARNING: possible undeclared group '@userlist' 
remote: Initialized empty Git repository in /local/git/repositories/examples/springmvc-thymeleaf-javaconfig.git/ 
To git@bsbrepo.bsb.lrz.de:/gitolite-admin.git 
   f535c57..0ff068e  master -> master
$
```

Now try again:

```sh
$ cd  minimvc-master
$ git push origin master
Counting objects: 30, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (16/16), done.
Writing objects: 100% (30/30), 11.67 KiB | 0 bytes/s, done.
Total 30 (delta 0), reused 0 (delta 0)
To git@bsbrepo.bsb.lrz.de:/examples/springmvc-thymeleaf-javaconfig.git
 * [new branch]      master -> master
$
```

## Adding repository description

```sh
$ ssh git@somereposerver.de desc examples/springmvc-thymeleaf-javaconfig "Example Webapp based on Spring MVC + Thymeleaf + Javaconfig"
```

## Checking the Status

Good job! As Git just told us, our octobox directory now has an empty repository in /.git/. The repository is a hidden directory where Git operates.
To save your progress as you go through this tutorial -- and earn a badge when you successfully complete it -- head over to create a free Code School account.
We'll wait for you here. Next up, let's type the git status command to see what the current state of our project is:

```sh
$ git status
# On branch master
#
# Initial commit
#
nothing to commit (create/copy files and use "git -add" to track)
Success!
```

The `.git` directory It's usually hidden but we're showing it to you for convenience.

If you click it you'll notice it has all sorts of directories and files inside it.
You'll rarely ever need to do anything inside here but it's the guts of Git, where all the magic happens.

## Adding & Committing

I created a file called octocat.txt in the octobox repository for you (as you can see in the browser below).

You should run the git status command again to see how the repository status has changed:

```sh
$ git status
# On branch master
#
# Initial commit
#
# Untracked files:
# (use „git add <file>..." to include in what will be committed)
#
# octocat.txt
nothing added to commit but untracked files present (use "git add" to track)
Success!
```

**Tip**: It's healthy to run git status often. Sometimes things change and you don't notice it.

### Adding Changes

Good, it looks like our Git repository is working properly. Notice how Git says octocat.txt is "untracked"?
That means Git sees that octocat.txt is a new file.
To tell Git to start tracking changes made to octocat.txt, we first need to add it to the staging area by using `git add`.

```sh
$ git add octocat.txt
```

Nice job, you've added octocat.txt to the Staging Area

**staged**: Files are ready to be committed.<br>
**unstaged**: Files with changes that have not been prepared to be commited.<br>
**untracked**: Files aren't tracked by Git yet. This usually indicates a newly created file.<br>
**deleted**: File has been deleted and is waiting to be removed from Git.

**add all**: You can also type `git add .`. The dot represents the current directory, so everything in it, and everything beneath it gets added.<br>
**git reset**: You can use `git reset <filename>` to remove a file or files from the staging area.

###  Checking for Changes

Good job! Git is now tracking our octocat.txt file. Let's run `git status` again to see where we stand:

```sh
$ git status
# On branch master
#
# Initial commit
#
# Changes to be committed:
# (use "git rm --cached <file>..." to unstage)
#
# new file:   octocat.txt
#
Success!
$
```

### Committing

Notice how Git says changes to be committed? The files listed here are in the Staging Area, and they are not in our repository yet.
We could add or remove files from the stage before we store them in the repository.
To store our staged changes we run the commit command with a message describing what we've changed. Let's do that now by typing:

```sh
$ git commit -m "Add cute octocat story"
[master (root-commit) aa74163] Add cute octocat story
 1 file changed, 1 insertion(+)
 create mode 100644 octocat.txt
Success!
```

**Staging Area**: A place where we can group files together before we "commit" them to Git.<br>
**Commit**: A "commit" is a snapshot of our repository. This way if we ever need to look back at the changes we've made (or if someone else does),
we will see a nice timeline of all changes.

### Adding All Changes

Great! You also can use wildcards if you want to add many files of the same type. Notice that I've added a bunch of .txt files into your directory below.
I put some in an octofamily directory and some others ended up in the root of our octobox.
Luckily, we can add all the new files using a wildcard with `git add`. Don't forget the quotes!

```sh
$ git add '*.txt'
Success!
$
```

**Wildcards**: We need quotes so that Git will receive the wildcard before our shell can interfere with it.
Without quotes our shell will only execute the wildcard search within the current directory.
Git will receive the list of files the shell found instead of the wildcard and it will not be able to add the files inside of the octofamily directory.

### Committing All Changes

Okay, you've added all the text files to the staging area. Feel free to run `git status` to see what you're about to commit.

If it looks good, go ahead and run:

```sh
$ git commit -m 'Add all the octocat txt files'
[master 220b6ec] Add all the octocat txt files
 4 files changed, 4 insertions(+)
 create mode 100644 blue_octocat.txt
 create mode 100644 octofamily/baby_octocat.txt
 create mode 100644 octofamily/momma_octocat.txt
 create mode 100644 red_octocat.txt
Success!
$
```

Check all the things!

When using wildcards you want to be extra careful when doing commits.
Make sure to check what files and folders are staged by using git status before you do the actual commit.
This way you can be sure you're committing only the things you want.

##  History

So we've made a few commits. Now let's browse them to see what we changed.
Fortunately for us, there's `git log`. Think of Git's log as a journal that remembers all the changes we've committed so far,
in the order we committed them. Try running it now:

```sh
$ git log
commit a33a3687428aa0ae5656908e54e33aab0b669cdf
Author: Try Git <try_git@github.com>
Date:   Tue Dec 18 07:51:51 2012 +0000

    Add all the octocat txt files

commit 4406775f68d4d6868da2ac6226f6c3074cd675c7
Author: Try Git <try_git@github.com>
Date:   Tue Dec 18 07:51:51 2012 +0000

    Added cute octocat story

Success!
```

More useful logs: Use `git log --summary` to see more information for each commit.
You can see where new files were added for the first time or where files were deleted.
It's a good overview of what's going on in the project.

So far we've only made commits to our local repository. Since we want to share our code,
we need to push it to a server so other people can pull down a copy of it.

GitHub makes this incredibly easy, so let's use them as our server.
You'll need a GitHub account to proceed with the rest of this course, so go ahead and sign in or create your GitHub account.
We'll wait here until you get back.

##  Cloning a remote repository (svn checkout)

See: <https://git.wiki.kernel.org/index.php/GitSvnCrashCourse>

Repository: <git@github.com:datazuul/marc-xml-javalib.git>

Netbeans:

* Team > Git > Clone... > git@github.com:datazuul/marc-xml-javalib.git, username/password
* Clone Repository, Remote Branches > master*
* Destination Directory: ~/Projects/github.com/datazuul
* Clone Name: marc-xml-javalib
* Checkout Branch: master*
* Remote Name: origin
* Scan for NetBeans Projects after Clone

## Branching

### Move uncommitted changes to own branch and reset current branches

```sh
$ git stash
Saved working directory and index state WIP on master: a6e35fd added getImage to JpegTranImageFactory
HEAD is now at a6e35fd added getImage to JpegTranImageFactory
$ git checkout -b "IIIF_2.0"
Switched to a new branch 'IIIF_2.0'
$ git stash pop
$ git commit -m “save it!”
$ git push -u origin IIIF_2.0
$ git checkout master
$ git clean -f
$ git reset -- hard
```

### Move committed changes to own branch and reset current branches

```sh
$ git checkout -b "BAV-85_CMS_Anlegen_eines_Themas"
```

### Delete Branch

```sh
$ git checkout master
$ git branch -d BAV-85 
Deleted branch BAV-85 (was 3356057).
```

## Rollback

### Rollback unstaged changes

Undo only unstaged changes:

```sh
git reset -- .
```

### Rollback commited and pushed changes

If you commited and pushed changes that you want to roll back

* search for the commit id in git log to which you want roll back
* roll back locally:

```sh
git reset --hard
```

or

```sh
git reset --hard <tag/branch/commit id>
```

* push rollback to remote:

```sh
git push origin master --force
```

## Adding a Local Git Repository to GitHub

See <http://berniecook.wordpress.com/2013/01/13/adding-a-local-git-repository-to-github-step-by-step-guide/>

Example: adding local Git-repository “com.datazuul.apps—datazuul-notepad” to GitHub

* Login to GitHub
  * Tab “Repositories” - Button “New”
  * Owner: datazuul, Repository name: com.datazuul.apps—datazuul-notepad, Public
  * Button “Create repository”
* Local

```sh
$ cd com.datazuul.apps--datazuul-notepad/
$ git remote add origin https://github.com/datazuul/com.datazuul.apps--datazuul-notepad.git 
$ git push -u origin master
Username for 'https://github.com': datazuul 
Password for 'https://datazuul@github.com': 
To https://github.com/datazuul/com.datazuul.apps--datazuul-notepad.git 
 * [new branch]      master -> master 
Branch master set up to track remote branch master from origin.
```

* Browse to <https://github.com/datazuul/com.datazuul.apps--datazuul-notepad>

## Moving One Git Repository Into Another

(Moving one Maven project as module into parent)

See <http://bpeirce.me/moving-one-git-repository-into-another.html>,
<http://www.nomachetejuggling.com/2011/09/12/moving-one-git-repo-into-another-as-subdirectory/>

```sh
$ git clone git@bsbrepo.bsb.lrz.de:/projects/bavarikon2-batchintegration.git bavarikon2-batchintegration_tmp
$ cd bavarikon2-batchintegration_tmp/
$ mkdir bavarikon2-batchintegration
$ mv !(bavarikon2-batchintegration) bavarikon2-batchintegration
$ mv .gitignore bavarikon2-batchintegration/
$ git add --all
$ git commit -m "Preparing project for move"
$ git push
$ git gc --aggressive
$ cd ../bavarikon2
$ git remote add temp ../bavarikon2-batchintegration_tmp/ 
$ git fetch temp
$ git merge temp/master
$ git add –all
$ git commit -m “merged batchintegration”
$ git push
$ git remote rm temp
```
