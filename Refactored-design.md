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

The central container will acquire some privileged relationship to Docker and running containers. It is unclear for now where the line will be drawn between reimplementing Docker-Compose features inside the central containers (sounds bad), using Docker Compose *as part of* the central container and keeping these details outside the central container.

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

How the configuration is generated and/or shared is still to be determined. An interesting model would be to have a configuration repository containing template files, where Jinja (or any other template engine) builds configuration files upon relevant configuration changes (defining "relevant" might be the challenge).

Then, built configuration directories could be mounted as volumes in the generic service containers.

Overriding configuration is a must have and all templates should be extensible and/or include override files.
