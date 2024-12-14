---
layout: post
title: Install Node.js and NPM (Node Package Manager)
author: Ralf Eichinger
toc: true
---

# Specification

* Homepage: <https://nodejs.org/>
* Version: 20 LTS
* Downloads: <https://nodejs.org/en/download>

# Installation

## Windows

Execute downloaded `node-v20.9.0-x64.msi` and install Node.js with NPM to your development tools directory.
The executables are also added to PATH, so you can test them after installation in a new terminal:

```
>node
Welcome to Node.js v20.9.0.
Type ".help" for more information.
> .exit

>npm --version
10.1.0
>
```

## Linux (Xubuntu)

```shell
$ sudo apt install nodejs
$ node -v
v18.13.0
$ sudo apt install npm
$ npm -v
9.2.0
```
