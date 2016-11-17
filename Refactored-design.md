This page summarizes a lot of ongoing discussions regarding long-term architectural plans for Mailu. As the project is gaining users and feature requests are pushing towards more and more hacking of configuration files, a proper design is required to address these requests in a flexible manner while keeping a simple and efficient model.

# Central container

The idea of having a central container is not new in Mailu: the admin container already centralizes most of the logics. However, data is exposed through multiple interfaces, which is a hassle to maintain: shared SQLite database, DKIM key files, authentication socket between Dovecot and Postfix, etc.

The communication model between containers is unclear and many changes break things (for instances, changing the running user for the fetchmail process broke error messages reporting because the SQLite file was not writeable).

It has become clear that we need a more self-contained central container that exposes one or more documented interfaces to the public.

## What interface(s)?

For now, the main required interface is an HTTP REST API. This API will be exposed for other containers to access data, but most of all for the Web administration interface to access its data.

A client library will be packaged and exposed as a wheel package directly downloadable.

## All in one container?

There is no plan to centralize all features in a single container. To the contrary, all current plans are going towards more generic and re-usable containers for Postfix, Dovecot and other services.

## What relationship to Docker and Docker Compose?

The central container exposes a public API. In the security model, it must not have any access to Docker. However, we need to be able to create, stop and restart containers, etc.

A worker container will acquire some privileged relationship to Docker and running containers. It will create the API container if none is available, then connect to the API to get the current configuration and start related containers. Upon configuration changes, it will restart required containers that might reload their configuration on the fly.

# Admin interface

The admin interface will access the REST API and serve a client-side application based on HTML and JavaScript. A command line admin interface could also be built using the same library and primitives.

# Configuration versionning

A lot of responsibilities will be offered to the central container, like maintaining all configurations, including maybe the Docker Compose configuration itself. Thus, all configurations should be versionned, with three mechanisms:

* the ability to quickly revert to a previous configuration state
* the ability to temporarily suspend configuration generation
* regular cleanups of old configurations (maybe set a maximum age or maximum number of configuration sets, or simply use Git)

Regarding the generation of new configuration versions, a timer should be set after each configuration change. A new version will be generated if configuration changes happen after the timer is out.

# Service containers

As opposed to the current model where all containers have very specific configuration, we could build really generic containers (even use some external ones) and generate configuration per-container.

Configuration will be generated using Jinja to allow for any configuration option to be used when configuring. It will be deployed over HTTP when containers start: the worker container will set a special entrypoint for every container with that purpose.

It will be possible to override any configuration file.