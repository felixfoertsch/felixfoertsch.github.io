---
layout: post
title: "Install Syncthing on Uberspace 6 on a Subdomain"
categories:
  - Tutorials
tags:
  - english
  - syncthing
  - sync
  - software
  - self-hosted
  - tutorial

last_modified_at: 2018-05-21
excerpt_separator: <!-- more -->
---

This short tutorial explains how to install [Syncthing](https://syncthing.net) on a [Uberspace](uberspace.de). It is based on the [Tutorial](https://maxhaesslein.de/dachboden/syncthing-auf-uberspace/) from Max Haesslein. Thanks Max!

## Download and Install Newest Syncthing Version

Connect to your uberspace:
```
$ ssh <user>@<uberspace>
```

Download the [newest Syncthing version](https://github.com/syncthing/syncthing/releases/latest):
```
$ cd ~/etc/  
$ wget <NEWEST AMD64 VERSION>
```

Extract the file (eXtract Ze Files!) and move it to its own folder:
```
$ tar -xzf <FILENAME>
$ mv <FILENAME> syncthing/
```

Link it to your binaries folder and start it once:
```
$ ln -s ~/etc/syncthing/syncthing ~/bin/syncthing
$ syncthing
```
Close the program with CTRL-C

## Prepare Uberspace

Get the ports we are going to use for GUI and sync:
```
$ uberspace-add-port -p tcp --firewall
🚀  All good! Opened tcp port <PORT1>.
$ uberspace-add-port -p both --firewall
🚀  All good! Opened tcp port <PORT2>.
```

Add a subdomain to make your Syncthing easily accessible; also make sure it is possible to connect through HTTPS:
```
$ uberspace-add-domain sync.<DOMAIN>.de
$ uberspace-letsencrypt
$ letsencrypt certonly
```
Don't forget to add the subdomain to your domain registrar DNS records aswell, so it gets redirected correctly!

Now create the corresponding folder on the Uberspace:
```
$ mkdir /var/www/virtual/<UBERSPACE>/sync.<DOMAIN>.de
```

Edit the `.htaccess` so it sends you to the correct port:
```
$ vim /var/www/virtual/<UBERSPACE>/sync.<DOMAIN>.de/.htaccess
```

```
RewriteEngine On
RewriteRule (.*) http://localhost:<PORT1>/$1 [P]
```

## Modify the Syncthing Config File

Find the GUI entry and replace the port with <PORT1>:
```
...
<gui enabled="true" tls="false" debugging="false">
  <address>127.0.0.1:<PORT1></address>
...
```

Find the options entry and replace the port with <PORT2>:
```
...
<options>
...
  <localAnnouncePort><PORT2></localAnnouncePort>
  <localAnnounceMCAddr>[ff12::8384]<PORT2></localAnnounceMCAddr>
...
```

## Setup Syncthing as a Service
If you don't have any services running, you have to setup the Daemon Tools first: [Uberspace Wiki, Daemon Tools](https://wiki.uberspace.de/system:daemontools)

After setting up the services, you can add Syncthing as a Service and start it:
```
$ uberspace-setup-service syncthing ~/bin/syncthing
$ svc -u ~/services/synthing
```

That's it! You now have a Syncthing service running on Uberspace! Ideal for private documents you don't want to have the NSA to have.
