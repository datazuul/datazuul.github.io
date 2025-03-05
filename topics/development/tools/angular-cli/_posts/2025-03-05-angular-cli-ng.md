---
layout: post
title: Installation and Usage of Angular CLI (ng)
author: Ralf Eichinger
toc: true
---

The Angular CLI is a command-line interface tool which allows you to scaffold, develop, test, deploy, and maintain Angular applications
 directly from a command shell.

Angular CLI is published on npm as the `@angular/cli` package and includes a binary named `ng`. Commands invoking `ng` are using the Angular CLI.

# Installation

Angular CLI uses Node and its package manager npm to install and run JavaScript tools outside the browser.

Before installing the Angular CLI [make sure Node.js + NPM are installed]({% post_url topics/development/tools/nodejs_npm/2025-03-05-javascript-tools-nvm-nodejs-npm %}). 

Afterwards, Angular CLI can be installed globally with

```sh
$ npm install -g @angular/cli

added 273 packages in 6s

52 packages are looking for funding
  run `npm fund` for details
```

To verify the installation, run `ng --version`.

```sh
$ ng --version
18.2.14
```

# Usage

## Show Angular CLI version

To show detailed version information, run `ng version`:

```sh
$ ng version

     _                      _                 ____ _     ___
    / \   _ __   __ _ _   _| | __ _ _ __     / ___| |   |_ _|
   / △ \ | '_ \ / _` | | | | |/ _` | '__|   | |   | |    | |
  / ___ \| | | | (_| | |_| | | (_| | |      | |___| |___ | |
 /_/   \_\_| |_|\__, |\__,_|_|\__,_|_|       \____|_____|___|
                |___/
    

Angular CLI: 18.2.14
Node: 22.14.0
Package Manager: npm 10.9.2
OS: linux x64

Angular: 18.2.13
... animations, common, compiler, compiler-cli, core, forms
... platform-browser, platform-browser-dynamic, router

Package                         Version
---------------------------------------------------------
@angular-devkit/architect       0.1802.14
@angular-devkit/build-angular   18.2.14
@angular-devkit/core            18.2.14
@angular-devkit/schematics      18.2.14
@angular/cli                    18.2.14
@schematics/angular             18.2.14
rxjs                            7.8.2
typescript                      5.4.5
zone.js                         0.14.10

```

## Create a new Angular project

### via command line

Angular apps are developed within an Angular workspace (= own directory).
To create a new workspace with an initial runnable skeleton for an app, run `ng new` with the name of the workspace/target directory and answer some questions.

Example with the following selection (default selections):

```sh
$ ng new my-app
? Which stylesheet format would you like to use? (Use arrow keys)
> CSS             [ https://developer.mozilla.org/docs/Web/CSS                     ]
  Sass (SCSS)     [ https://sass-lang.com/documentation/syntax#scss                ]
  Sass (Indented) [ https://sass-lang.com/documentation/syntax#the-indented-syntax ]
  Less            [ http://lesscss.org                                             ]
? Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)? (y/N)
CREATE my-app/angular.json (2686 bytes)
CREATE my-app/package.json (1075 bytes)
CREATE my-app/README.md (1093 bytes)
CREATE my-app/tsconfig.json (1054 bytes)
CREATE my-app/.editorconfig (290 bytes)
CREATE my-app/.gitignore (629 bytes)
CREATE my-app/tsconfig.app.json (439 bytes)
CREATE my-app/tsconfig.spec.json (449 bytes)
CREATE my-app/.vscode/extensions.json (134 bytes)
CREATE my-app/.vscode/launch.json (490 bytes)
CREATE my-app/.vscode/tasks.json (980 bytes)
CREATE my-app/src/main.ts (256 bytes)
CREATE my-app/src/index.html (304 bytes)
CREATE my-app/src/styles.css (81 bytes)
CREATE my-app/src/app/app.component.html (20239 bytes)
CREATE my-app/src/app/app.component.spec.ts (945 bytes)
CREATE my-app/src/app/app.component.ts (315 bytes)
CREATE my-app/src/app/app.component.css (0 bytes)
CREATE my-app/src/app/app.config.ts (318 bytes)
CREATE my-app/src/app/app.routes.ts (80 bytes)
CREATE my-app/public/favicon.ico (15086 bytes)
/ Installing packages (npm)...
- Packages installed successfully.
    Directory is already under version control. Skipping initialization of git.
```

The Angular CLI installs the necessary Angular npm packages and other dependencies. This can take a few minutes.

The CLI creates a new workspace and a small welcome app in a new directory `my-app` with the same name as the workspace, ready to run.
Navigate to the new directory so subsequent commands use this workspace.

### via IntelliJ

IntelliJ provides an integrated project dialog. To create a new Angular project/workspace click `File - New - Project - Angular CLI`.
This will create a project skeleton with default selections.

### Fix configuration

If you get on running the app with `ng serve` the error "Error: NG0203: inject() must be called from an injection context such as a constructor, a factory function, a field initializer, or a function used with `runInInjectionContext`", this might be caused by your system using symlinks / mapping for the directory the project is located.

Fix this in file `angular.json` of the generated project (add `"preserveSymlinks": true`):

```json
"projects": {
    "my-app": {
      "architect": {
        "build": {
          "options": {
            "preserveSymlinks": true,
```

## Run Angular app

The Angular CLI includes a development server, for you to build and serve your app locally. Run the command `ng serve --open` in your project's workspace/directory:

```sh
$ cd my-app
$ ng serve --open
 Would you like to enable autocompletion? This will set up your terminal so pressing TAB while typing Angular CLI commands will show possible options and autocomplete arguments. (Enabling autocompletion will modify configuration files in your home 
directory.) yes
Appended `source <(ng completion script)` to `/home/ralf/.bashrc`. Restart your terminal or run the following to autocomplete `ng` commands:

    source <(ng completion script)
 
Would you like to share pseudonymous usage data about this project with the Angular Team
at Google under Google's Privacy Policy at https://policies.google.com/privacy. For more
details and how to change this setting, see https://angular.dev/cli/analytics.

   no
Global setting: not set
Local setting: disabled
Effective status: disabled
Initial chunk files | Names         |  Raw size
scripts.js          | scripts       |   1.44 MB | 
main.js             | main          | 528.40 kB | 
styles.css          | styles        | 393.03 kB | 
polyfills.js        | polyfills     |  92.97 kB | 

                    | Initial total |   2.45 MB

Application bundle generation complete. [2.962 seconds]

▲ [WARNING] Polyfill for "@angular/localize/init" was added automatically. [plugin angular-polyfills]

  In the future, this functionality will be removed. Please add this polyfill in the "polyfills" section of your "angular.json" instead.


Watch mode enabled. Watching for file changes...
NOTE: Raw file sizes do not reflect development server per-request transformations.
  ➜  Local:   http://localhost:4200/
  ➜  press h + enter to show help
h

  Shortcuts
  press r + enter to force reload browser
  press u + enter to show server url
  press o + enter to open in browser
  press c + enter to clear console
  press q + enter to quit
```

On first usage you are asked to activate code completion for `ng` (Yes) and if you want to share pseudonymous usage data.

The `ng serve` command launches the server, watches your files, as well as rebuilds the app and reloads the browser as you make changes to those files.

The `--open` (or just `-o`) option automatically opens your browser to <http://localhost:4200/> to view the generated application.

You are ready and set to develop Angular apps!

---

References:

* <https://angular.dev/tools/cli>
* <https://angular.dev/tools/cli/setup-local>