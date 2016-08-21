Installing Freeposte.io
=======================

Things to consider
==================

Freeposte.io is working, it has been powering hundreds of e-mail accounts
since around January 2016. It is still not massively tested however and
you should not run any critical mail server until you have properly tested
every feature.

Also, the idea behind Freeposte.io is based on the work by folks from Poste.io.
If free software is not the reason you chose Freeposte.io or if you are seeking
long-term professional support, you should probably turn to them instead.

Preparing the mail server environment
=====================================

Freeposte.io will store all of its persistent data in ``/freeposte`` by default,
simply create the directory and move there:

```
mkdir /freeposte
cd /freeposte
```

Docker Compose configuration is stored in a file named ``docker-compose.yml``.
Additionally, Freeposte.io relies on an environment file for various settings.

Download the templates files from the git repository:

```
wget https://raw.githubusercontent.com/kaiyou/freeposte.io/master/docker-compose.yml
wget https://raw.githubusercontent.com/kaiyou/freeposte.io/master/freeposte.env
```

These templates are used for development environment. So, if you do not plan
on building Freeposte.io from source, simply remove the ``build:`` references:

```
sed -i '/build:/d' docker-compose.yml
```

The default configuration will pull the latest stable image built from the Docker
Hub, which is based on the latest stable commit on GitHub. This behaviour is ok for
most setups. If you wish to specify a branch (avoid including breaking changes
unintendidly for instance), you will still get bugfixes and security updates until
the next stable release is replaced, but breaking changed will not be pulled unless
you explicitely change the branch number. To specify you want to pull the ``1.0``
branch, simply add the version number to the ``image`` field.

If you wish to use the testing version, simply set the Docker tag at ``testing``
instead of a branch number. You should always have all containers using the same
branch (especially, never mix testing with any stable branch).

Finally, edit the ``freeposte.env`` file and update the following settings:

 - set ``DEBUG`` to ``False`` unless your are debugging,
 - set ``SECRET_KEY`` to a random 16 bytes string,
 - set ``DOMAIN`` to your main mail domain,
 - set ``ADMIN`` to the local part of the admin address on the main domain,
 - set ``HOSTNAME`` to your mailserver hostname.

Setting up certificates
=======================

Freeposte.io relies heavily on TLS and must have a key pair and a certificate
available, at least for the hostname configured in ``freeposte.env``.

Create the certificate directory:

```
mkdir /freeposte/certs
```

Then create two files in this directory:

 - ``cert.pem`` contains the certificate,
 - ``key.pem`` contains the key pair.

Creating the first admin user
=============================

Freeposte.io does not come with any default user. You have to create the
first admin user manually. First, start the mail server stack:

```
docker-compose up -d
```

Then create the admin user:

```
docker exec -i -t freeposte_admin_1 python manage.py admin admin example.net admin
```

This will create ``admin@example.net`` with password ``admin``. Connect to
the Web admin interface change the password to a strong one:

```
https://your-host-name.tld/admin/
```

Testing before going live
=========================

You should test all the critical features before using the mail server for
actual messages. Try to send and receive e-mails, monitor the logs for some
unexpected errors, etc.

Your server should now be running!