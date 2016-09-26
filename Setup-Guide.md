Things to consider
==================

Freeposte.io is working, it has been powering hundreds of e-mail accounts
since around January 2016. It is still not massively tested however and
you should not run any critical mail server until you have properly tested
every feature.

Also, the idea behind Freeposte.io is based on the work by folks from Poste.io.
If free software is not the reason you chose Freeposte.io or if you are seeking
long-term professional support, you should probably turn to them instead.

Prepare the environment
=======================

You are free to choose any operating system that runs Docker (>= 1.11) and has
the latest Docker Compose version installed. Some instructions are provided on
the matter in the article [[Setup a base server]].

Freeposte.io will store all of its persistent data in a path of your choice
(``/freeposte`` by default) simply create the directory and move there:

```
mkdir /freeposte
cd /freeposte
```

Docker Compose configuration is stored in a file named ``docker-compose.yml``.
Additionally, Freeposte.io relies on an environment file for various settings.
Download the latest template files from the git repository:

```
wget https://raw.githubusercontent.com/kaiyou/freeposte.io/master/docker-compose.yml
wget https://raw.githubusercontent.com/kaiyou/freeposte.io/master/.env
```

Then open the ``.env`` file to setup the mail server. Modify the ``ROOT`` setting
to match your setup directory if different from ``/freeposte``.

Pick a version
==============

Freeposte.io is shipped in multiple versions.

- ``stable`` is the default version and features a stable server, it gets bugfixes
  and patches but features are included every month or so after a couple weeks of
  testing. This is the default setting and should match most requirements.

- ``1.0``, ``1.1``, and other version branches feature old versions of Freeposte.io,
  they will not receive any more patches (except for the stable one) and you should
  not remain forever on one of those branches; you could however setup the stable
  branch by number to avoid introducing unexpected new feature until you read the
  changelog properly. This is the most conservative option.

- ``latest`` points at the latest build from the master
  development branch. It will most likely contain many more bugs, also you should
  never use it for a production server. You are more than welcome to run a testing
  server however and report bugs.

Pick one of these versions and modify the ``VERSION`` configuration in the ``.env``
file.

Set the common configuration values
===================================

You *must* modify ``SECRET_KEY`` and set it to a randomly generated 16-bytes (16
characters) value. Be very careful when generating the secret key. You should never
disclose it to anyone either as it would compromise most of your mail server
security.

Modify ``BIND_ADDRESS`` to match the public IP address assigned to your server.
This address should be configured on one of the network interfaces of the server.
If the address is not configured directly (NAT) on any of the network interfaces or if
you would simply like the server to listen on all interfaces, use ``0.0.0.0``.

Modify ``DOMAIN`` to match your main email domain. This could be anyone of the
domains that you plan on serving and will be used when building special (admin)
email addresses and in some protocols that require a domain is explicitely
exposed.

Set ``ADMIN`` to the localpart of the admin address that you plan on creating
on the main domain. For instance, if you main domain is ``domain.tld`` and your
admin localpart is ``admin``, then administrative emails will be sent to
``admin`domain.tld``. This address might also be exposed on some services
(mailer daemon notifications, error pages, etc.) You can later create a mailbox
or an alias for that address.

Set ``HOSTNAME`` to match the public hostname of your mail server. This must
be a fully qualified domain name. All your services (IMAP, SMTP, Web interface,
etc.) will be available using that hostname. If you request a TLS certificate,
it must match the hostname.

Enable optional features
========================

Some of Freeposte.io are not used by every user and are thus not enabled in a
default configuration.

A Webmail is a Web interface exposing an email client. Freeposte.io webmails are
bound to the internal IMAP and SMTP server for users to access their mailbox through
the Web. By exposing a complex application such as a Webmail, you should be aware of
the security implications such an increase of attack surface. The ``WEBMAIL``
configuration option must be one of the following:

- ``none`` is the default value, no Webmail service will be exposed;
- ``roundcube`` will run the popular Roundcube Webmail.

If you plan on exposing a Webmail or if you would like to expose the administration
interface on the public server address, you should set the ``FRONTEND`` configuration
variable properly:

- ``none`` is the default value, no Web interface will be exposed;
- ``nginx`` will expose a nginx-based reverse proxy forwarding to ``/admin`` to the
  administration interface and exposing a Webmail if any is configured.

Starting with version ``1.3``, the administration interface is not exposed on the
public address by default, even
with a frontend configured, you still need to set the ``EXPOSE_ADMIN`` variable:

- ``yes`` will expose the admin interface in ``/admin``;
- ``no`` (or any other value) will disable this behaviour.

Install certificates
=====================

Freeposte.io relies heavily on TLS and must have a key pair and a certificate
available, at least for the hostname configured in the ``.env`` file.

Create the certificate directory:

```
mkdir /freeposte/certs
```

Then create two files in this directory:

 - ``cert.pem`` contains the certificate,
 - ``key.pem`` contains the key pair.

Start Freeposte.io
==================

You may now start Freeposte.io. Move the to the Freeposte.io directory and run:

```
docker-compose up -d
```

Before you start using Freeposte.io, you must create an admin user:

```
docker-compose run --rm admin python manage.py admin root example.net password
```

This will create a user named ``root@example.net`` with password ``password`` and administration privileges. Connect to the Web admin interface and change the password to a strong one.

Test before going live
======================

You should test all the critical features before using the mail server for
actual messages. Try to send and receive e-mails, monitor the logs for some
unexpected errors, etc.

Your server should now be running!