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

last_modified_at:
excerpt_separator: <!-- more -->
---

This tutorial explains how to install [Mailtrain](https://mailtrain.org) on a [Uberspace 7](uberspace.de). [Mailtrain](https://mailtrain.org/) is a self-hosted open-source (released under the [GPL
v3.0](https://github.com/Mailtrain-org/mailtrain/blob/master/LICENSE).) newsletter app built on top of [Nodemailer](https://nodemailer.com/). I am following the [manual installation guide](https://github.com/Mailtrain-org/mailtrain#quick-start---manual-install-any-os-that-supports-nodejs) from the official Mailtrain repo and add some additional Uberspace infos. I contributed this guide to the [Uberlab](lab.uberspace.de)---see [here](https://github.com/Uberspace/lab/issues/430)---but at the point of writing, it has not yet been merged.

<!-- more -->

## Installation

This guide uses Node.js version 12, which is the [default](https://manual.uberspace.de/lang-nodejs.html#standard-version) on Uberspace 7 at at the moment.

Clone the [GitHub](https://github.com/Mailtrain-org/mailtrain)
repository:

```console
[isabell@stardust ~]$ git clone git://github.com/Mailtrain-org/mailtrain.git
[isabell@stardust ~]$
```

Install the required dependencies:

```console
[isabell@stardust ~]$ cd mailtrain
[isabell@stardust mailtrain]$ npm install --production
[isabell@stardust mailtrain]$
```

## Configuration

### Database Setup

Create a new database:

```console
[isabell@stardust mailtrain]$ mysql -e "CREATE DATABASE ${USER}_mailtrain;"
[isabell@stardust mailtrain]$
```

### Mailtrain Config

Copy the example config file:

```console
[isabell@stardust mailtrain]$ cp config/default.toml config/production.toml
[isabell@stardust mailtrain]$
```

Update `production.toml` with your MySQL settings; look for the
`[mysql]` block:

```console
...

[mysql]
host="localhost"
user="isabell"
password="MySuperSecretPassword"
database="isabell_mailtrain"

...
```

### Web Backend Config

[Mailtrain](https://mailtrain.org/) is running on port 3000. Configure the server to respond to port 3000 using web backends:

``` console
[isabell@stardust ~]$ uberspace web backend set / --http --port 3000
[isabell@stardust ~]$
```

### Supervisord Daemon Setup

Create `~/etc/services.d/mailtrain.ini` with the following content:

```ini
[program:mailtrain]
directory=%(ENV_HOME)s/mailtrain/
command=env NODE_ENV=production /bin/node index.js
autostart=yes
autorestart=yes
```

If it's not in state RUNNING, check your configuration.

### Login and Change Admin Credentials

{% include bootstrap/alert.html 
  content="Change the default admin credentials to prevent unauthorized access of your data!"
  style="danger"
%}

Your [Mailtrain](https://mailtrain.org/) installation should now be
reachable on `https://isabell.uber.space`. Log in with the username
`admin` and the password `test`.

Go to `https://isabell.uber.space/users/account` and change your email
address as well as your password.

{% include bootstrap/alert.html 
  content="It is not possible to change the username in the GUI. If you want to change the default username `admin` to something else or add additional users, you have to do it directly in the database."
  style="info"
%}

## Finishing installation

This guide contains only the required settings that enable full Mailtrain functionality. Change the rest of the settings according to your requirements.

Go to `https://isabell.uber.space/settings`.

In the **General Settings** section change the **Service Address (URL)**
to `https://isabell.uber.space/`.

In the **Mailer Settings** section change the

> -   *Hostname* to `stardust.uberspace.de`,
> -   *Port* to `587`,
> -   *Encryption* to
>     `Use STARTTLS - usually selected for port 587 and 25`,
> -   *username* to `isabell@uber.space`,
> -   *password* to `MySuperSecretPassword` and
> -   test your settings by pressing the Button **Check Mailer Config**.

{% include bootstrap/alert.html 
  content="Uberspace discourages **mass** mailings. If you regularly send a large amount of emails, consider using [AWS SES](https://aws.amazon.com/ses/) instead of the Uberspace mailing system."
  style="warning"
%}

## Best Practices

Test the configuration by creating a new list and subscribing yourself
to it.

------------------------------------------------------------------------

Tested on Uberspace v7.7.0 with NodeJS v12 and MariaDB 10.3.23.
