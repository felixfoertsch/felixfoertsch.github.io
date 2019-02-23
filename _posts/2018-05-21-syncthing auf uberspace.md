---
layout: post
title: "Install Syncthing on Uberspace 6 on a Subdomain"
categories:
  - Tutorials
tags:
  - english
  - software
  - self-hosted
  - sync

last_modified_at: 2018-07-03
excerpt_separator: <!-- more -->
---

This short tutorial explains how to install [Syncthing](https://syncthing.net) on a [Uberspace](https://uberspace.de). It is based on the [~~Tutorial~~](https://maxhaesslein.de/dachboden/syncthing-auf-uberspace/) from [Max Haesslein](http://maxhaesslein.blog). Thanks Max!
<!-- more -->

## Download newest Syncthing version

Connect to your uberspace:
```
ssh <user>@<uberspace>
```

Download the [newest Syncthing version](https://github.com/syncthing/syncthing/releases/latest) for Linux:
```
cd ~/etc/  
wget <NEWEST AMD64 LINUX VERSION>
```

Extract the file (eXtract Ze Files!) and move it to its own folder:
```
tar -xzf <FILENAME>
mv <FOLDERNAME> syncthing/
```

Link it to your binaries folder and start it once:
```
ln -s ~/etc/syncthing/syncthing ~/bin/syncthing
syncthing
```
Close the program with CTRL-C.


## Prepare your Uberspace

Get the ports we are going to use for GUI and sync:
```
uberspace-add-port -p tcp --firewall
🚀  All good! Opened tcp port <PORT1>.
uberspace-add-port -p both --firewall
🚀  All good! Opened tcp port <PORT2>.
```

Add a subdomain to make your Syncthing easily accessible; also make sure it is possible to connect through HTTPS:
```
uberspace-add-domain -w -d sync.<DOMAIN>.de
uberspace-letsencrypt
uberspace-letsencrypt-renew -f
```
If you already use LetsEncrypt, you have to add the domain to your configuration. Just follow the steps displayed after the `uberspace-letsencrypt`. Don't forget to add the subdomain to your domain registrar DNS records aswell, so it gets redirected correctly!

Now, create the corresponding folder on the Uberspace:
```
mkdir /var/www/virtual/<UBERSPACE>/sync.<DOMAIN>.de
```

Edit the `.htaccess` so it sends you to the correct port:
```
cd /var/www/virtual/<UBERSPACE>/sync.<DOMAIN>.de/
vim .htaccess
```

```
RewriteEngine On
RewriteRule (.*) http://localhost:<PORT1>/$1 [P]
```


## Modify the Syncthing config file

Open the syncthing config file:
```
cd ~/.config/syncthing
vim config.xml
```

Find the GUI entry and replace the port with `<PORT1>`:
```
...
<gui enabled="true" tls="false" debugging="false">
  <address>127.0.0.1:<PORT1></address>
...
```

Find the options entry and replace the port with `<PORT2>`:
```
...
<options>
...
  <localAnnouncePort><PORT2></localAnnouncePort>
  <localAnnounceMCAddr>[ff12::8384]<PORT2></localAnnounceMCAddr>
...
```


## Setup Syncthing as a service

If you don't have any services running, you have to setup the Daemon Tools first: [Uberspace Wiki, Daemon Tools](https://wiki.uberspace.de/system:daemontools).

After setting up the services, you can add Syncthing as a service and restart it once:
```
uberspace-setup-service syncthing ~/bin/syncthing
svc -du ~/services/syncthing
```


## Secure your Syncthing instance

Your Syncthing is now available via `sync.<DOMAIN>.de`. However, it is accessible by everyone on the web. Therefore you should secure it. Got to the Syncthing Settings:
![You can access your settings via Actions -> Settings](/img/20180602-Syncthing-settings.png)

**IMPORTANT: Do not check `Use HTTPS for GUI` you will lose access to your GUI. To get it back you have to find the TLS option in your Syncthing config and set it to `false` again. You connections will have TLS and will be secure without checking this box.**

And now enter your admin user and password in the corresponding fields:
![You can access your settings via Actions -> Settings](/img/20180602-Syncthing-settings2.png)




## The end

Save and that's it! You now have a Syncthing service running on Uberspace! Ideal for private documents you don't want the NSA to have.
