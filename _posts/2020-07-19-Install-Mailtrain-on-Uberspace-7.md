---
layout: post
title: "Install Mailtrain on Uberspace 7"
categories:
  - Tutorials
tags:
  - English
  - Self-Hosted Software

last_modified_at: 2020-07-21
excerpt_separator: <!-- more -->
---

This tutorial explains how to install [Mailtrain](https://mailtrain.org) on a [Uberspace 7](uberspace.de). [Mailtrain](https://mailtrain.org/) is a self-hosted open-source (released under the [GPL
v3.0](https://github.com/Mailtrain-org/mailtrain/blob/master/LICENSE).) newsletter app built on top of [Nodemailer](https://nodemailer.com/). I am following the [manual installation guide](https://github.com/Mailtrain-org/mailtrain#quick-start---manual-install-any-os-that-supports-nodejs) from the official Mailtrain repo and add some additional Uberspace infos. I contributed [this guide](https://lab.uberspace.de/guide_mailtrain.html) to the [Uberlab](https://lab.uberspace.de/) and earned my first [Ubercup](https://github.com/Uberspace/lab/blob/master/CONTRIBUTING.md#reward).

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

Go to `https://isabell.uber.space/settings`. In the **General Settings** section change the **Service Address (URL)**
to `https://isabell.uber.space/`.

In the **Mailer Settings** section change the

> -   *Hostname*,
> -   *Port*,
> -   *Encryption*,
> -   *username*,
> -   *password*, and
> -   test your settings by pressing the Button **Check Mailer Config**.

{% include bootstrap/alert.html 
  content="Uberspace does not allow mass mailings from their servers according to their House Rules. However, you can use Mailtrain as the admin interface for your mailing needs. Use the SMTP services from AWS SES, Sendgrid, Mailgun, etc. for the actual mailing."
  style="warning"
%}

## Best Practices

* Test the configuration by creating a new list and subscribing yourself
to it.
* Craft your campaign with love and dedication.
* Don’t spam users that don’t want your newsletter.

------------------------------------------------------------------------

Tested on Uberspace v7.7.0 with NodeJS v12 and MariaDB 10.3.23.
