---
layout: post
title: "Install Mailtrain on Uberspace 7"
categories:
  - Tutorials
tags:
  - English
  - email
  - mailtrain
  - mailinglist
  - software
  - self-hosted
  - tutorial

last_modified_at: 2018-06-2
excerpt_separator: <!-- more -->
---

This short tutorial explains how to install [Mailtrain](https://mailtrain.org) on a [Uberspace](uberspace.de). I am following the [manual installation guide](https://github.com/Mailtrain-org/mailtrain#quick-start---manual-install-any-os-that-supports-nodejs) from the official Mailtrain repo and add some additional Uberspace infos.
<!-- more -->


## Download newest Mailtrain version

Connect to your uberspace:
```
ssh <user>@<uberspace>
```

Clone the newest Mailtrain version:
```
cd ~/etc/  
git clone git://github.com/Mailtrain-org/mailtrain.git
```


## Get Node ready on your Uberspace

Mailtrain uses Node.js with a version greater than 7. Since we are on a shared hosting service, we want to add our personal home directory as prefix (copy and paste the whole code block):
```
cat > ~/.npmrc <<__EOF__
> prefix = $HOME
> umask = 077
> __EOF__
```

Depending on your Uberspace, the default Node version might be really old. You can see all installed versions of your Uberspace with the following command:
```
ls -ld /package/host/localhost/nodejs-*
```

To set it to a different version, in our case version 8, do the following:
```
echo 'export PATH=/package/host/localhost/nodejs-8/bin:$PATH' >> ~/.bash_profile
```

In case you worked with node before and encounter erros during installation, you should check your `~/.bash_profile` for entries that look like the one from the above code box. If you delete node versions from your profile, make sure to `source` it afterwards!

npm install npm@latest -g

## Install Mailtrain

Fetch all the dependencies with `npm`:
```
cd mailtrain
npm install --production
```
## Get a port for your installation

Get the ports we are going to use for GUI and sync:
```
uberspace-add-port -p tcp --firewall
🚀  All good! Opened tcp port <PORT1>.
```
Write that port down, you will need it later.


## Add a database that Mailtrain can store its data

Since Mailtrain needs an MySQL database to store its infortmation, we are going to create one:
```
mysql -e "CREATE DATABASE ${USER}_mailtrain CHARACTER SET utf8"
```

If you don't know your MySQL username and password, this is the time to write them down:
```
cat ~/.my.cnf
```
Make sure to write down the correct password in the `[client]` section! The other password is read only.

## Configure Mailtrain

There are two things we have to configure:

1. The database information
2. The application port

Since we want to future-proof our configuration, we are following the advice from the default configuration file to not edit it directly, but create our own config. Therefore we will be able to update Mailtrain with simple `git` operations.

```
cd ~/etc/mailtrain/config
vim production.toml
```

Copy and paste the following and enter the information you gathered in the steps before into the placeholders:
```
# production.toml
[www]
port=<PORT>

[mysql]
host="localhost"
user="<MYSQL-USER>"
password="<MYSQL-PASSWORD>"
database="<MYSQL-USER>_mailtrain"
port=3306
charset="utf8"
```


## Add subdomain for the server
Since we want to run Mailtrain on a subdomain -- in my case on `mt.<DOMAIN>.de`, we have to add that domain to our server. Additionally we want to use TLS to make sure our service is secure, thus we will be using LetsEncrypt:
```
uberspace-add-domain -w -d mt.<DOMAIN>.de
uberspace-letsencrypt
uberspace-letsencrypt-renew -f
```

If you already use LetsEncrypt, you have to add the domain to your configuration. Just follow the steps displayed after the `uberspace-letsencrypt` Don't forget to add the subdomain to your domain registrar DNS records aswell, so it gets redirected correctly!

Now create the corresponding folder on the Uberspace:
```
mkdir /var/www/virtual/<UBERSPACE>/mt.<DOMAIN>.de
```

Edit the `.htaccess` so it sends you to the correct port:
```
cd /var/www/virtual/<UBERSPACE>/mt.<DOMAIN>.de/
vim .htaccess
```

```
RewriteEngine On
RewriteRule (.*) http://localhost:<PORT>/$1 [P]
```


## Setup Mailtrain as a service

If you don't have any services running, you have to setup the Daemon Tools first: [Uberspace Wiki, Daemon Tools](https://wiki.uberspace.de/system:daemontools).

After setting up the services, you can add Syncthing as a Service and restart it once:
```
uberspace-setup-service mailtrain ~/bin/syncthing
svc -du ~/services/syncthing
```



## The End

That's it! You now have a Syncthing service running on Uberspace! Ideal for private documents you don't want the NSA to have.
