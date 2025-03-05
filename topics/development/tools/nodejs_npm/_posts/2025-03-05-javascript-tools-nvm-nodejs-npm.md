---
layout: post
title: Installation and Usage of Javascript Developer Tools NVM, Node.js (v22), NPM (v10)
author: Ralf Eichinger
toc: true
---

# Introduction

For frontend development with Javascript frameworks (like Angular, React, etc.) some basic tools have to be installed.

* **Node.js + npm** \
    `Node.js` is a Javascript runtime environment (like JRE for Java) and `npm` is the "Node Package Manager" for Javascript packages (similar to Maven and Gradle for Java).
  * Homepage: <https://nodejs.org/>
  * Documentation:
    * Node.js: <https://nodejs.org/en/learn/getting-started/how-to-install-nodejs>
    * npm: <https://nodejs.org/en/learn/getting-started/an-introduction-to-the-npm-package-manager>

Although there are options to install Node.js via an installer (https://nodejs.org/en/download/prebuilt-installer) or as a ZIP (https://nodejs.org/en/download/prebuilt-binaries), it is recommended to install Node.js (and the bundled npm) via NVM. This makes it easier to switch between Node.js versions and to install new versions (additionally).

* **NVM** \
    NVM is the Node Version Manager to install/use/switch multiple versions of Node.js + npm.
  * Homepage: <https://github.com/nvm-sh/nvm> (for Linux), <https://github.com/coreybutler/nvm-windows> (for Windows)
  * Downloads: <https://github.com/nvm-sh/nvm/releases> (for Linux), <https://github.com/coreybutler/nvm-windows/releases> (for Windows)

NOTE: A successor tool is being worked on: "Runtime", see <https://github.com/coreybutler/nvm-windows/wiki/Runtime>

# Installation

The following steps describe the installation of NVM and NVM controlled Node.js and NPM.

## Remove previously installed Node.js and NPM

### Windows

Delete Node.js directory and remove path to directory from `PATH` environment variable.

### Debian Linux

Uninstall installed node.js + npm.

```sh
$ sudo apt list --installed nodejs
nodejs/oracular,now 20.16.0+dfsg-1ubuntu1 amd64  [Installiert,automatisch]
$ sudo apt autoremove nodejs
```

## Install NVM

### Windows

* Download latest installer `nvm-setup.exe` from <https://github.com/coreybutler/nvm-windows/releases>, e.g. <https://github.com/coreybutler/nvm-windows/releases/download/1.2.2/nvm-setup.exe>
* Execute `nvm-setup.exe` install to a directory of your choice, e.g. `D:\dev\nvm`
* The setup sets environment variables, it is advisable to reboot so that all node/npm paths work.

### Debian Linux

To install or update nvm, you should run the install script. Download and run the script manually or use the following cURL or Wget command:

```sh
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
...
=> Appending nvm source string to ~/.bashrc
=> Appending bash_completion source string to ~/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.config/nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

or Wget:

```sh
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

Running either of the above commands downloads the install script and runs it. The script clones the nvm repository to `~/.config/nvm` (because `$XDG_CONFIG_HOME` is set to `~/.config`, and attempts to add the source lines from the snippet (`export ...`) to the correct profile file (`~/.bashrc`, `~/.bash_profile`, `~/.zshrc`, or `~/.profile`). If you find the install script is updating the wrong profile file, set the `$PROFILE` env var to the profile file's path, and then rerun the installation script.

Reload changed profile file, e.g. for bash:

```sh
$ source ~/.bashrc
```

To verify that nvm has been installed, do:

```sh
$ command -v nvm
nvm
$ nvm -v
0.40.1
```

Which should output "nvm" if the installation was successful. Please note that `which nvm` will not work, since `nvm` is a sourced shell function, not an executable binary.

# Usage of NVM (install and use Node.js and NPM)

NOTE for Windows: nvm-windows runs in an Admin shell. You'll need to start powershell or Command Prompt as Administrator to use nvm-windows

## Show currently used Node.js version

```sh
$ nvm current
none
```

## Install a version of Node.js and NPM

NOTE: If you are behind a proxy, you have to set it with:

```sh
$ nvm proxy http://your_proxy:port
```

To install the latest Node.js version you issue the command `nvm install latest` (specific versions can be installed using the version number, e.g. `nvm install 14.7.0`). To be more conservative we install the last LTS (Long Tem Support) version:

```sh
$ nvm install --lts
Installing latest LTS version.
Downloading and installing node v22.14.0...
Downloading https://nodejs.org/dist/v22.14.0/node-v22.14.0-linux-x64.tar.xz...
############################################################################################################ 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v22.14.0 (npm v10.9.2)
Creating default alias: default -> lts/* (-> v22.14.0)
```

Checking successful installation and activation:

```sh
$ nvm current
v22.14.0
$ npm -v
10.9.2
$ node -v
v22.14.0
```

## Remotely available Node.js versions

To list all (remotely) available Node.js versions, issue:

```sh
$ nvm ls-remote
        v0.1.14
        v0.1.15
        ...
       v22.13.1   (LTS: Jod)
->     v22.14.0   (Latest LTS: Jod)
        v23.0.0
        ...
        v23.8.0
        v23.9.0
```

You see that the whole history of Node.js is available! The currently used version ("v22.14.0   (Latest LTS: Jod)") is marked with an arrow. See <https://nodejs.org/en/about/previous-releases> for Node.js releases documentation.

## Locally available Node.js versions

To list all locally available versions (should be only one, the just installed one):

```sh
$ nvm ls
->     v22.14.0
default -> lts/* (-> v22.14.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v22.14.0) (default)
stable -> 22.14 (-> v22.14.0) (default)
lts/* -> lts/jod (-> v22.14.0)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.2 (-> N/A)
lts/hydrogen -> v18.20.7 (-> N/A)
lts/iron -> v20.18.3 (-> N/A)
lts/jod -> v22.14.0
```

Ok, a lot more information as expected. We see that our current version `v22.14.0` is also aliased as `default`, `node`, `stable` and `lts/jod`.

## Use a Node.js version (switch/activate)

To use a (previously installed, locally available) version of Node.js (switch to it), issue `nvm use ...`:

```sh
$ nvm use 22.14.0
Now using node v22.14.0 (npm v10.9.2)
```

# NPM (Node Package Manager)

NPM takes care of downloading all dependencies of a project (similar to Maven/Gradle).

## Proxy Configuration

If you are behind a proxy server, you have to configure it for NPM.

Create file `.npmrc` in your home directory:

```ini
proxy=http://your_proxy:port/
http_proxy=http://your_proxy:port/
http-proxy=http://your_proxy:port/
https_proxy=http://your_proxy:port/
https-proxy=http://your_proxy:port/
noproxy=localhost, ...
```

## Package Management

### Install all project's dependencies

If a javascript project contains a `package.json` file, all dependencies can be downloaded with:

```sh
$ npm install

up to date, audited 1023 packages in 2s

154 packages are looking for funding
  run `npm fund` for details

11 vulnerabilities (4 moderate, 7 high)

To address issues that do not require attention, run:
  npm audit fix

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

The dependencies are downloaded to project's subdirectory `node_modules`.

### Install and Add a new package to project

To install a new package (= add dependency to project):

```sh
$ npm install <package-name>
```

Following options can be added to command:

* _--save-dev_ (oder _-D_): installs and adds the entry to the package.json file devDependencies (only development tools like testing library, not bundled for production)
* _--no-save_: installs but does not add the entry to the package.json file dependencies
* _--save-optional_ (oder _-O_): installs and adds the entry to the package.json file optionalDependencies
* _--no-optional_: will prevent optional dependencies from being installed

### Update packages

To update all packages' versions:

```sh
$ npm update

added 50 packages, removed 56 packages, changed 240 packages, and audited 1016 packages in 34s

162 packages are looking for funding
  run `npm fund` for details

5 moderate severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

To update a single package:

```sh
$ npm update <package-name>
```

---

References:

* For Windows: <https://github.com/coreybutler/nvm-windows?tab=readme-ov-file#installation--upgrades>
* For Linux: <https://github.com/nvm-sh/nvm?tab=readme-ov-file#installing-and-updating>
* NPM: https://nodejs.org/en/learn/getting-started/an-introduction-to-the-npm-package-manager