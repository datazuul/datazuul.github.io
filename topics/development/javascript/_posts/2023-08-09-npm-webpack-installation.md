---
layout: post
title: Installation and usage of webpack with npm (and nvm)
author: Ralf Eichinger
toc: true
---

If you want to build a javascript project using webpack and get the following error:

```
$ npm run webpack

> mirador-integration@0.0.0 webpack
> webpack --config webpack/webpack.config.js

sh: 1: webpack: not found
```

`webpack` is not installed.

To install it globally (for all users and all projects), do this:

```
$ sudo su -
# npm install -g webpack

changed 77 packages in 724ms

9 packages are looking for funding
  run `npm fund` for details
#
```

You might get another error message:

```
$ npm run webpack

> mirador-integration@0.0.0 webpack
> webpack --config webpack/webpack.config.js

CLI for webpack must be installed.
  webpack-cli (https://github.com/webpack/webpack-cli)

We will use "npm" to install the CLI via "npm install -D webpack-cli".
Do you want to install 'webpack-cli' (yes/no):
```

Just quit with `no` and install it again globally:

```
$ sudo su -
# npm install -g webpack-cli

added 117 packages in 2s

15 packages are looking for funding
  run `npm fund` for details
#
```

And before executing `webpack` we have to do an `install`:

```
$ npm install
$ npm run webpack

> mirador-integration@0.0.0 webpack
> webpack --config webpack/webpack.config.js

node:internal/crypto/hash:71
  this[kHandle] = new _Hash(algorithm, xofLen);
                  ^

Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:71:19)
    at Object.createHash (node:crypto:133:10)
    at module.exports (/home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/webpack/lib/util/createHash.js:135:53)
    at NormalModule._initBuildHash (/home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/webpack/lib/NormalModule.js:417:16)
    at handleParseError (/home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/webpack/lib/NormalModule.js:471:10)
    at /home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/webpack/lib/NormalModule.js:503:5
    at /home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/webpack/lib/NormalModule.js:358:12
    at /home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/loader-runner/lib/LoaderRunner.js:373:3
    at iterateNormalLoaders (/home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/loader-runner/lib/LoaderRunner.js:214:10)
    at Array.<anonymous> (/home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/loader-runner/lib/LoaderRunner.js:205:4)
    at Storage.finished (/home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/enhanced-resolve/lib/CachedInputFileSystem.js:55:16)
    at /home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/enhanced-resolve/lib/CachedInputFileSystem.js:91:9
    at /home/ralf/development/github.com/ProjectMirador/mirador-integration/node_modules/graceful-fs/graceful-fs.js:123:16
    at FSReqCallback.readFileAfterClose [as oncomplete] (node:internal/fs/read_file_context:68:3) {
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}

Node.js v18.16.1
```

If you get this error: your `npm` version is too new :-)

So we install `nvm` to be able to switch between different `npm` versions:

To install or update nvm, you should run the install script (see <https://github.com/nvm-sh/nvm#installing-and-updating>). To do that, you may either download and run the script manually, or use the following cURL or Wget command:

```
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
=> nvm is already installed in /home/ralf/.nvm, trying to update using git
=> => Compressing and cleaning up git repository

=> nvm source string already in /home/ralf/.bashrc
=> bash_completion source string already in /home/ralf/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

To use it in current terminal, we execute

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Test it:

```
$ nvm --version
0.39.4
```

Usage of `nvm` to switch to different `node` / `npm` version (see <https://github.com/nvm-sh/nvm#usage>):

Check what version is currently in use:

```
$ nvm run node --version
Running node v20.1.0 (npm v9.6.6)
v20.1.0
```

Install latest `node` version:

```
$ nvm install node
Downloading and installing node v20.5.0...
Downloading https://nodejs.org/dist/v20.5.0/node-v20.5.0-linux-x64.tar.xz...
######################################################################### 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v20.5.0 (npm v9.8.0)
```

List available versions:

```
$ nvm ls-remote
        v0.1.14
        v0.1.15
         ...
         v4.9.1   (Latest LTS: Argon)
         v5.0.0
         v5.1.0
         ...
       v18.17.0   (Latest LTS: Hydrogen)
        v19.0.0
        v19.0.1
        v19.1.0
        v19.2.0
        v19.3.0
        v19.4.0
        v19.5.0
        v19.6.0
        v19.6.1
        v19.7.0
        v19.8.0
        v19.8.1
        v19.9.0
        v20.0.0
        v20.1.0
        v20.2.0
        v20.3.0
        v20.3.1
        v20.4.0
->      v20.5.0
```

Install a specific version (`v18.17.0`):

```
$ nvm install 18.17.0
Downloading and installing node v18.17.0...
Downloading https://nodejs.org/dist/v18.17.0/node-v18.17.0-linux-x64.tar.xz...
##################################################################################################################################################################### 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v18.17.0 (npm v9.6.7)
```

Use specific version

```
$ nvm use 18.17.0
Now using node v18.17.0 (npm v9.6.7)
```

List local versions:

```
$ nvm ls
        v8.17.0
       v16.20.0
->     v18.17.0
        v20.0.0
        v20.1.0
        v20.5.0
         system
default -> lts/gallium (-> N/A)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v20.5.0) (default)
stable -> 20.5 (-> v20.5.0) (default)
lts/* -> lts/hydrogen (-> v18.17.0)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3 (-> N/A)
lts/gallium -> v16.20.1 (-> N/A)
lts/hydrogen -> v18.17.0
```

Now we are using version 18.17.0 and tried again - no success.
So we install node 16:

```
$ nvm install 16
Downloading and installing node v16.20.1...
Downloading https://nodejs.org/dist/v16.20.1/node-v16.20.1-linux-x64.tar.xz...
##################################################################################################################################################################### 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v16.20.1 (npm v8.19.4)
```

Try again:

```
$ nvm use 16
Now using node v16.20.1 (npm v8.19.4)
$ npm run webpack
.... ( a lot of warnings but no error!) ....
$
```

Hooray! No we have a result (packaged project):

```
$ ls -al webpack/dist/
insgesamt 2032
drwxrwxr-x 2 ralf ralf    4096 Aug  9 15:13 .
drwxrwxr-x 3 ralf ralf    4096 Aug  9 15:13 ..
-rw-rw-r-- 1 ralf ralf  341005 Aug  9 15:13 1.main.js
-rw-rw-r-- 1 ralf ralf  254076 Aug  9 15:13 2.main.js
-rw-rw-r-- 1 ralf ralf   15569 Aug  9 15:13 3.main.js
-rw-rw-r-- 1 ralf ralf    3253 Aug  9 15:13 4.main.js
-rw-rw-r-- 1 ralf ralf    2739 Aug  9 15:13 5.main.js
-rw-rw-r-- 1 ralf ralf   10664 Aug  9 15:13 6.main.js
-rw-rw-r-- 1 ralf ralf    2756 Aug  9 15:13 7.main.js
-rw-rw-r-- 1 ralf ralf 1427209 Aug  9 15:13 main.js
```

Finally the result is `main.js`.