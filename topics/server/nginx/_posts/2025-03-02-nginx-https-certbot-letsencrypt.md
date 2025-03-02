---
layout: post
title: Securing a nginx website (HTTPS/SSL) with Letsencrypt and Certbot
author: Ralf Eichinger
toc: true
---

Following our previous post [nginx Webserver - Installation, Configuration, Operations]({% post_url topics/server/nginx/2025-03-02-nginx-webserver %}) we configured a nginx webserver serving websites under HTTP (port 80).

In this post we will configure our system to secure the websites with a SSL certificate to enable SSL/HTTPS (port 443) using certbot and letsencrypt.

See: <https://letsencrypt.org/>, <https://letsencrypt.org/getting-started/>, <https://certbot.eff.org/>

Certbot-Website:

<https://certbot.eff.org/instructions>, <https://certbot.eff.org/lets-encrypt/debianstretch-nginx>

 
# Installation

See <https://certbot.eff.org/instructions?ws=nginx&os=snap> for recommended installation using snap.

Following installation instructions are for Debian 12.

## SSH into the server

SSH into the server running your HTTP website as a user with sudo privileges.

## Install snapd

You'll need to install snapd and make sure you follow any instructions to enable classic snap support.

```sh
$ sudo apt update
$ sudo apt install snapd
...
The following additional packages will be installed:
  dbus-user-session liblzo2-2 squashfs-tools
Suggested packages:
  zenity | kdialog
The following NEW packages will be installed:
  dbus-user-session liblzo2-2 snapd squashfs-tools
...
$ sudo snap install snapd
2025-03-02T16:05:01Z INFO Waiting for automatic snapd restart...
snapd 2.67 from Canonical✓ installed
```

## Test snapd

To test your system, install the hello-world snap and make sure it runs correctly:

```sh
$ sudo snap install hello-world
hello-world 6.4 from Canonical✓ installed
```

Logout from server and login again to refresh PATH settings.

Test `hello-world`:

```sh
$ hello-world
Hello World!
```

## Remove certbot-auto and any Certbot OS packages

If you have any Certbot packages installed, you should remove them before installing the Certbot snap to ensure that when you run the command certbot the snap is used rather than the installation from your OS package manager. 

```sh
$ sudo apt-get remove certbot
[sudo] password for datazuul: 
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Package 'certbot' is not installed, so not removed
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## Install Certbot

Run this command on the command line on the machine to install Certbot.

```sh
$ sudo snap install --classic certbot
certbot 3.2.0 from Certbot Project (certbot-eff✓) installed
```

## Prepare the Certbot command

Execute the following instruction on the command line on the machine to ensure that the certbot command can be run.

```sh
$ sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

# Configuration

## Get and install your certificates

Run this command to get a certificate and have Certbot edit your nginx configuration automatically to serve it, turning on HTTPS access in a single step.

```sh
$ sudo certbot --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): your@email.tld

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at:
https://letsencrypt.org/documents/LE-SA-v1.5-February-24-2025.pdf
You must agree in order to register with the ACME server. Do you agree?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: N
Account registered.

Which names would you like to activate HTTPS for?
We recommend selecting either all domains, or all domains in a VirtualHost/server block.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: munhattan.com
2: www.munhattan.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 
Requesting a certificate for munhattan.com and www.munhattan.com

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/munhattan.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/munhattan.com/privkey.pem
This certificate expires on 2025-05-31.
These files will be updated when the certificate renews.
Certbot has set up a scheduled task to automatically renew this certificate in the background.

Deploying certificate
Successfully deployed certificate for munhattan.com to /etc/nginx/conf.d/www.munhattan.com.conf
Successfully deployed certificate for www.munhattan.com to /etc/nginx/conf.d/www.munhattan.com.conf
Congratulations! You have successfully enabled HTTPS on https://munhattan.com and https://www.munhattan.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

Certbot automatically configures your domains and has set up a a scheduled task to automatically renew this certificate in the background.

## Virtual Host configuration

Certbot automatically changed the configuration file for the selected virtual hosts.

Example `/etc/nginx/conf.d/www.munhattan.com.conf`:

```sh
server {
    server_name  www.munhattan.com munhattan.com;

    ...

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/munhattan.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/munhattan.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server {
    if ($host = www.munhattan.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = munhattan.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen       80;
    server_name  www.munhattan.com munhattan.com;
    return 404; # managed by Certbot
}
```

It added a configuration section for port 443 (HTTPS/SSL) and added redirect rules to redirect all calls to HTTP to HTTPS address.

## renew timer / scheduled task

The schedulded tasks can be listed:

```sh
$ systemctl list-timers
NEXT                        LEFT          LAST                        PASSED       UNIT                         ACTIVATES                     
Sun 2025-03-02 19:17:00 UTC 2h 56min left -                           -            snap.certbot.renew.timer     snap.certbot.renew.service
...

9 timers listed.
Pass --all to see loaded but inactive timers, too.
```

The configuration of the timer for certbot renewal is located under `/etc/systemd/system/snap.certbot.renew.timer`:

```sh
$ less /etc/systemd/system/snap.certbot.renew.timer
[Unit]
# Auto-generated, DO NOT EDIT
Description=Timer renew for snap application certbot.renew
Requires=snap-certbot-4412.mount
After=snap-certbot-4412.mount
X-Snappy=yes

[Timer]
Unit=snap.certbot.renew.service
OnCalendar=*-*-* 09:21
OnCalendar=*-*-* 19:17

[Install]
WantedBy=timers.target
```

The renewal is started every day at 09:21 and 19:17 UTC time.

The task is configured under `/etc/systemd/system/snap.certbot.renew.service`:

```sh
$ less /etc/systemd/system/snap.certbot.renew.service
[Unit]
# Auto-generated, DO NOT EDIT
Description=Service for snap application certbot.renew
Requires=snap-certbot-4412.mount
Wants=network.target
After=snap-certbot-4412.mount network.target snapd.apparmor.service
X-Snappy=yes

[Service]
EnvironmentFile=-/etc/environment
ExecStart=/usr/bin/snap run --timer="00:00~24:00/2" certbot.renew
SyslogIdentifier=certbot.renew
Restart=no
WorkingDirectory=/var/snap/certbot/4412
TimeoutStopSec=30
Type=oneshot
```

## Test automatic renewal

The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire. You will not need to run Certbot again, unless you change your configuration. You can test automatic renewal for your certificates by running this command:

```sh
$ sudo certbot renew --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/munhattan.com.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Account registered.
Simulating renewal of an existing certificate for munhattan.com and www.munhattan.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations, all simulated renewals succeeded: 
  /etc/letsencrypt/live/munhattan.com/fullchain.pem (success)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

## Confirm that Certbot worked

To confirm that your site is set up properly, visit https://yourwebsite.com/ in your browser and look for the lock icon in the URL bar.

