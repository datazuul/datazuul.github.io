---
layout: post
title: Angular Javascript Frontend Framework - Part 1 (Setup)
author: Ralf Eichinger
toc: true
---

* Homepage: <https://angular.io/>
* Documentation: <https://angular.io/docs>

# What is Angular?

> "Angular is an application-design framework and development platform for creating efficient and sophisticated single-page apps."

# Development Environment

See documentation at <https://angular.io/tutorial/first-app#before-you-start>

## node.js and npm

Required versions for Angular 15.1.5: see <https://unpkg.com/browse/@angular/core@15.1.5/package.json>

```
"engines": {
  "node": "^14.20.0 || ^16.13.0 || >=18.10.0"
}
```

Installed or old?

```
$ node --version
v16.20.0
```

On 2023/05/10: Latest Current Version: 20.1.0 (includes npm 9.6.4)

If not installed or old: Install it following <https://nodejs.org/en/download> or <https://nodejs.org/en/download/package-manager> (pointing to <https://github.com/nodesource/distributions#installation-instructions>)

Linux (Debian/Ubuntu):

```
$ curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
...
## Run `sudo apt-get install -y nodejs` to install Node.js 20.x and npm
...
nodejs (20.1.0-deb-1nodesource1) wird eingerichtet ...
...
$ node --version
v20.1.0
```

Check installation of `npm` (Node Package manager):

```
$ npm --version
9.6.4
```

## Install the latest version of Angular

> "With node.js and npm installed, the next step is to install the Angular CLI which provides tooling for effective Angular development."

Install Angualr tools with `-g` flag (system-wide to be used by all system users):

```
$ sudo npm install -g @angular/cli
added 256 packages in 8s

37 packages are looking for funding
  run `npm fund` for details
npm notice 
npm notice New patch version of npm available! 9.6.4 -> 9.6.6
npm notice Changelog: https://github.com/npm/cli/releases/tag/v9.6.6
npm notice Run npm install -g npm@9.6.6 to update!
npm notice
```

Ok, we get a notice for a new patch version of npm... let's update it:

```
$ sudo npm install -g npm@9.6.6

changed 51 packages in 1s

27 packages are looking for funding
  run `npm fund` for details
```

Check version of installed Angualar CLI tool using the `ng` executable:

```
$ ng version
? Would you like to enable autocompletion? This will set up your terminal so pressing TAB while typing Angular CLI commands will show possible
 options and autocomplete arguments. (Enabling autocompletion will modify configuration files in your home directory.) Yes
Appended `source <(ng completion script)` to `/home/ralf/.bashrc`. Restart your terminal or run the following to autocomplete `ng` commands:

    source <(ng completion script)
? Would you like to share pseudonymous usage data about this project with the Angular Team
at Google under Google's Privacy Policy at https://policies.google.com/privacy. For more
details and how to change this setting, see https://angular.io/analytics. No
Global setting: disabled
Local setting: No local workspace configuration file.
Effective status: disabled

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/
    

Angular CLI: 16.0.0
Node: 20.1.0 (Unsupported)
Package Manager: npm 9.6.6
OS: linux x64

Angular: 
... 

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.1600.0 (cli-only)
@angular-devkit/core         16.0.0 (cli-only)
@angular-devkit/schematics   16.0.0 (cli-only)
@schematics/angular          16.0.0 (cli-only)
    
Warning: The current version of Node (20.1.0) is not supported by Angular.
```

`ng version` started something bigger... Ok, we agree to enable autocompletion but for me I decided not to share usage data.

Unfortunately in the end it is stated that `Node (20.1.0) is not supported by Angular.` (even if fulfills the reuirement `>=18.10.0`).

Ok, so we downgrade to the latest LTS version 18.16.0 (includes npm 9.5.1) and install Angular CLI again:

```
$ sudo apt remove nodejs
$ sudo rm -rf /usr/lib/node_modules

$ sudo apt update
$ sudo apt install snapd
$ sudo snap install node --classic
node (18/stable) 18.16.0 aus OpenJS Foundation (iojs✓) installiert

$ node --version
v18.16.0

$ npm --version
9.5.1

$ sudo npm install -g @angular/cli
npm WARN deprecated @npmcli/move-file@2.0.1: This functionality has been moved to @npmcli/fs

added 256 packages in 6s

37 packages are looking for funding
  run `npm fund` for details

```

Now the version check shows what we want to see:

```
$ ng version

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/
    

Angular CLI: 16.0.0
Node: 18.16.0
Package Manager: npm 
OS: linux x64

Angular: 
... 

Package                      Version
------------------------------------------------------
@angular-devkit/architect    0.1600.0 (cli-only)
@angular-devkit/core         16.0.0 (cli-only)
@angular-devkit/schematics   16.0.0 (cli-only)
@schematics/angular          16.0.0 (cli-only)
```

## Install integrated development environment (IDE)

> You are free to use any tool you prefer to build apps with Angular. We recommend the following:
> - Visual Studio Code <https://code.visualstudio.com/>
> - Angular Language Service <https://marketplace.visualstudio.com/items?itemName=Angular.ng-template>

We download and install VS Code following <https://code.visualstudio.com/docs/setup/linux>:

```
$ sudo apt install ./code_1.78.1-1683194560_amd64.deb
$ code --version
1.78.1
6a995c4f4cc2ced6e3237749973982e751cb0bf9
x6
```

Optionally read <https://code.visualstudio.com/docs/setup/linux#_next-steps>

Now we install the Angular Language Service in VS Code following <https://marketplace.visualstudio.com/items?itemName=Angular.ng-template>:

* Open VS Code by calling `code` in terminal
* Launch VS Code Quick Open (Ctrl+P), paste `ext install Angular.ng-template`, and press enter.

For further development of Angular apps: To get the most complete information in the editor, set the strictTemplates option in tsconfig.json, as shown in the following example:

```
"angularCompilerOptions": {
  "strictTemplates": true
}
```

For more information, see the Angular compiler options guide <https://angular.io/guide/angular-compiler-options>.

# Summary

In this first part of our Angular series we installed all necessary tools to get started.
In the ["next part"]({% post_url topics/development/javascript/2023-05-10-angular-part2-hello_world %}) we are going to develop our first Angular webapp.


