---
layout: post
title: Angular Javascript Frontend Framework - Part 2 (Hello World)
author: Ralf Eichinger
toc: true
---


# Introduction

This tutorial recaps the official documentation at <https://angular.io/tutorial/first-app/first-app-lesson-01>.

We will not really start from scratch but download a basic skeleton app and add functionality step by step.

# Download skeleton

Download skeleton app from <https://angular.io/generated/zips/first-app-lesson-00/first-app-lesson-00.zip> and unzip it into a directory named `angular-hello-world`.

```
angular-hello-world$ ls -al
insgesamt 44
drwxrwxr-x  4 ralf ralf 4096 Mai 10 10:07 .
drwxrwxr-x 13 ralf ralf 4096 Mai 10 10:07 ..
-rw-r--r--  1 ralf ralf 3047 Dez 31  1979 angular.json
drwx------  3 ralf ralf 4096 Mai 10 10:07 e2e
-rw-r--r--  1 ralf ralf  274 Dez 31  1979 .editorconfig
-rw-r--r--  1 ralf ralf  548 Dez 31  1979 .gitignore
-rw-r--r--  1 ralf ralf 1381 Dez 31  1979 package.json
drwx------  4 ralf ralf 4096 Mai 10 10:07 src
-rw-r--r--  1 ralf ralf  587 Dez 31  1979 tsconfig.app.json
-rw-r--r--  1 ralf ralf  901 Dez 31  1979 tsconfig.json
-rw-r--r--  1 ralf ralf  273 Dez 31  1979 tsconfig.spec.json
```

# Test the default app

## Build the default Angular app

```
$ cd angular-hello-world
$ npm install
npm ERR! code ERESOLVE
npm ERR! ERESOLVE unable to resolve dependency tree
npm ERR! 
npm ERR! While resolving: angular.io-example@0.0.0
npm ERR! Found: @angular/common@16.0.0
npm ERR! node_modules/@angular/common
npm ERR!   @angular/common@"^16.0.0" from the root project
npm ERR! 
npm ERR! Could not resolve dependency:
npm ERR! peer @angular/common@"^15.0.0" from angular-in-memory-web-api@0.15.0
npm ERR! node_modules/angular-in-memory-web-api
npm ERR!   angular-in-memory-web-api@"~0.15.0" from the root project
npm ERR! 
npm ERR! Fix the upstream dependency conflict, or retry
npm ERR! this command with --force or --legacy-peer-deps
npm ERR! to accept an incorrect (and potentially broken) dependency resolution.
npm ERR! 
npm ERR! 
npm ERR! For a full report see:
npm ERR! /home/ralf/.npm/_logs/2023-05-10T08_18_00_302Z-eresolve-report.txt

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/ralf/.npm/_logs/2023-05-10T08_18_00_302Z-debug-0.log
```

Unfortunately again a version problem...
We execute with mentioned option to circumvent this:

```
$ npm install --legacy-peer-deps
npm WARN deprecated @npmcli/move-file@2.0.1: This functionality has been moved to @npmcli/fs
npm WARN deprecated har-validator@5.1.5: this library is no longer supported
npm WARN deprecated uuid@3.4.0: Please upgrade  to version 7 or higher.  Older versions may use Math.random() in certain circumstances, which is known to be problematic.  See https://v8.dev/blog/math-random for details.
npm WARN deprecated request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
npm WARN deprecated protractor@7.0.0: We have news to share - Protractor is deprecated and will reach end-of-life by Summer 2023. To learn more and find out about other options please refer to this post on the Angular blog. Thank you for using and contributing to Protractor. https://goo.gle/state-of-e2e-in-angular

added 1079 packages, and audited 1080 packages in 24s

104 packages are looking for funding
  run `npm fund` for details

6 moderate severity vulnerabilities

To address all issues (including breaking changes), run:
  npm audit fix --force

Run `npm audit` for details.
```

For now we do not fix issues as we do not know if this would break the example app...

## Build and serve the default app

```
$ ng serve
✔ Browser application bundle generation complete.

Initial Chunk Files   | Names         |  Raw Size
vendor.js             | vendor        |   1.94 MB | 
polyfills.js          | polyfills     | 328.83 kB | 
styles.css, styles.js | styles        | 226.62 kB | 
runtime.js            | runtime       |   6.53 kB | 
main.js               | main          |   3.62 kB | 

                      | Initial Total |   2.49 MB

Build at: 2023-05-10T08:30:37.509Z - Hash: 6113c937375038b3 - Time: 5804ms

** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


✔ Compiled successfully.
```

The app is now up and running, open <http://localhost:4200> in the browser:

![Hello World!](/assets/topics/development/javascript/angular-hello-world-01.jpg)

Not really exciting :-) but we have a successful running Angular app (showing a "Default" h1 heading...)!

For the next steps you can leave `ng serve` running.

# Review the files in the project

Open the project folder in VS Code with `File - Open Folder...`:

Directory tree after above successful build:

![Directory structure](/assets/topics/development/javascript/angular-hello-world-02.jpg)

## Directory `src`

Contains the Angular app's files.

* `index.html` is the app's top level HTML template.
* `style.css` is the app's top level style sheet.
* `main.ts` is where the app starts running.
* `favicon.ico` is the app's icon, just as you would find in web site.

## Directory `app`

Contains the Angular app's component files.

* `app.component.ts` is the source file that describes the app-root component. This is the top-level Angular component in the app. A component is the basic building block of an Angular application. The component description includes the component's code, HTML template, and styles, which can be described in this file, or in separate files. In this app, the styles are in a separate file while the component's code and HTML template are in this file.
* `app.component.css` is the style sheet for this component.

New components are added to this directory.

## Directory `assets`

Contains images used by the app.

## Other files and directories

The other files and directories the Angular app needs to build and run, but they are not files that you normally interact with.

* `.angular` has files required to build the Angular app.
* `.e2e` has files used to test the app.
* `.node_modules` has the node.js packages that the app uses.
* `angular.json` describes the Angular app to the app building tools.
* `package.json` is used by npm (the node package manager) to run the finished app.
* `tsconfig.*` are the files that describe the app's configuration to the TypeScript compiler.

# Create Hello World

Now we will update the Angular project files to change the displayed content.

## File `src/index.html`

Change app's title:

```
<title>Homes</title>
```

## File `src/app/app.component.ts`

Change component's template property:

```
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [],
  template: `<h1>Hello world!</h1>`,
  styleUrls: ['./app.component.css'],
})
```

(Make sure to use backticks for working javascript templating)

Change the component's title to 'homes':

```
export class AppComponent {
  title = 'homes';
}
```

The changes should now be visible at <http://localhost:4200> in the browser:

![Hello World!](/assets/topics/development/javascript/angular-hello-world-03.jpg)