
Upgrading the mail server
=========================

First check upstream for changes in the ``docker-compose.yml`` or in the
``.env`` files. Also, check ``CHANGELOG.md`` for changes that you
might not want to include.

Update your ``.env`` file to reflect the version that you wish to install (if
you are running ``stable`` or ``latest``, you may skip this and proceed), then
simply pull the latest images and recreate the containers :

```
docker-compose pull
docker-compose up -d
```

Monitoring the mail server
==========================

Logs are managed by Docker directly. You can easily read your logs using :

```
docker-compose logs
```

Docker is able to forward logs to multiple log engines. Read the following documentation or details: https://docs.docker.com/engine/admin/logging/overview/.