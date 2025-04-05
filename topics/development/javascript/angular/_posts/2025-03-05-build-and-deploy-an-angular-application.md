---
layout: post
title: Build and Deploy an Angular Application to a NGINX Webserver
author: Ralf Eichinger
toc: true
---

Your Angular application is locally running and tested.
You are ready to deploy your Angular application to a remote server.

To manually deploy your application, create a production build and copy the output directory to a web server or content delivery network (CDN).
By default, `ng build` uses the `production` configuration.

If you have customized your build configurations, you may want to confirm production optimizations are being applied before deploying.

`ng build` outputs the built artifacts into the subdirectory `dist/my-app/` in your workspace/project's directory by default, however this path can be configured with the `outputPath` option in the `@angular-devkit/build-angular:browser` builder.

Copy build artifacts to the server and configure your webserver to serve the directory.

# Build application

```sh
Initial chunk files   | Names         |  Raw size | Estimated transfer size
main-KP3ZWSOV.js      | main          |   1.49 MB |               336.81 kB
scripts-OCZFKTZ6.js   | scripts       |   1.28 MB |               386.89 kB
styles-7MDPNQY7.css   | styles        | 340.08 kB |                36.54 kB
polyfills-MH2GNY63.js | polyfills     |  35.81 kB |                11.77 kB

                      | Initial total |   3.15 MB |               772.02 kB

Application bundle generation complete. [7.669 seconds]

...
(... warnings ...)

Output location: ~/.../your_app/dist/your_app
```

The directory contains a file `3rdpartylicenses.txt` with collected third party packages licenses and the application itself in the subdirectory `browser`:

```sh
$ ls dist/your_app/*
dist/your_app/3rdpartylicenses.txt

dist/your_app/browser:
assets  index.html  main-KP3ZWSOV.js  media  polyfills-MH2GNY63.js  scripts-OCZFKTZ6.js  styles-7MDPNQY7.css
```

# Configure NGINX for Angular application

## Basic authentication

If you deploy an early (not ready for public usage) version of your application,
you can hide the application behind a simple username/password (basic) authentication.

To configure basic authentication for your website install `htpasswd` contained in Debian package `apache2-utils`
and create a username / password pair:

```sh
$ sudo apt install apache2-utils
$ sudo htpasswd -c /etc/nginx/.htpasswd your_username
New password: 
Re-type new password: 
Adding password for user your_username
```

For further users omit the `-c`("create") flag because file already exists then.

Add basic auth to your website's virtual host configuration and reload nginx:

```sh
$ sudo nano /etc/nginx/conf.d/www.your-website.tld.conf
server {
    ...

    auth_basic           "Login required";
    auth_basic_user_file /etc/nginx/.htpasswd;

    ...
}
$ sudo service nginx reload
```

Your Angular application is only accessible after basic authentication now.

## Configure Angular routing

TODO

https://angular.dev/tools/cli/deployment#server-configuration

# Manual deployment to server

Manual deployment can be done by simply copying build artifacts to remote server using `scp`.

Before doing this make sure your user has write access to target directory, e.g.:

```sh
$ sudo usermod -a -G nginx your_remote_username
$ sudo chmod -R g+w /var/www/www.your-website.tld/htdocs/
```

Now we can start copying files:

```sh
$ cd dist/myapp/browser/
$ scp -r * your_remote_username@your-website.tld:/var/www/www.your-website.tld/htdocs/
```

---

References:

* Angular deployment: <https://angular.dev/tools/cli/deployment>
* NGINX Basic authentication: <https://docs.nginx.com/nginx/admin-guide/security-controls/configuring-http-basic-authentication/>
* Angular Server configuration: <https://angular.dev/tools/cli/deployment#server-configuration>