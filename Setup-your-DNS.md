E-mail has a decentralized architecture: pretty much every e-mail provider on the Internet has the ability to communicate with every other provider to deliver e-mails, much like post-office services from different cities or countries communicate to deliver standard mail.

The analogy stops when it comes to discovery and responsibilities: in the e-mail model, there most of the time two, maximum three actors in delivering a message: the emitter, an (optional) relay and the recipient server (some architectures are much more complex, but either they can be simplified for the purpose of this document or they are extremely rare). Forgetting about the relay, the emitter has to discover where the recipient server is located around the Internet, which is based on DNS.

Setting up a DNS domain is (almost) mandatory to exchange e-mail on the Internet. Before you start using Mailu, you must have at least one domain setup to receive emails.

The mail server hostname
========================

Your mail server will have a unique hostname. That hostname is a fully qualified domain name, that points to your server IP address. How you pick the hostname is up to you, it must of course belong to a domain name that you own or manage and could belong to a domain that your server will receive mail for.

You should pick a meaningful hostname that you can give your users to access the Web interface and other email services. For instance, if your main mail domain is ``mydomain.com`` (the one you setup in ``DOMAIN`` in your configuration file), then you could use ``mail.mydomain.com`` as a mail server hostname.

Set that name in the ``HOSTNAME`` configuration entry. Then depending on your domain provider, make sure that you have an address record (``A``) serving the public IP address of your server:

```
mail.mydomain.com.  IN  A  a.b.c.d
```

Also, ``a.b.c.d`` should be set in your ``BIND_INTERFACE`` configuration unless your server is in a DMZ and you are using port forwards to expose the services.

