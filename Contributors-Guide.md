Development environment
=======================

The development environment is quite similar to the production one. Simply specify the ``:testing``
tag on images that you do not build locally and build the others using ``docker-compose``:

```
docker-compose build
```

Web administration
==================

The administration Web interface requires a proper dev environment that can easily be setup using ``virtualenv`` (make sure you are using Python 3) :

```
cd admin
virtualenv .
source bin/activate
pip install -r requirements.txt
```

You can then export the path to the development database:

```
export SQLALCHEMY_DATABASE_URI=sqlite:///path/to/dev.db
```

And finally run the server with debug enabled:

```
python manage.py runserver
```

Generating migration scripts
============================

After you modify the database models, you need to generate a new schema
migration script:

```
python manage.py db migrate
```

This will generate a new script in ``migrations/versions`` that you must review
before committing it.

Philosophy
==========

The mailserver is designed as a whole, some images are therefore not best
suited for reuse outside this project. All images should however follow
Docker best practices and be as generic as possible :

 - even if not suited for reuse, they should be simple enough to
   fit as base images for other projects,
 - interesting settings should be available as environment variables
 - base images should be well-trusted (officiel Alpine or Debian for instance).

Git workflow
============

Forking vs. committing
----------------------

Linus' way of things sounds fine for now: if you find yourself implementing a
new feature, either send me an email with a bunch of commits or use Github
pull request feature. Maybe if the user base grows too much as well as the need
for trust in a specific branch of the project, we can switch to a shared
repository and add a couple of trusted committers.

Commits
-------

This is a community project, thus commits should be readable enough for any of
the contributors to guess the content by simply reading the comment or find a
proper commit when one knows what he is looking for.

Usual standards remain: write english comments, single line short comments and
additional multiline if required (keep in mind that the most important piece
of information should fit in the first line).

Branches
--------

You are of course free of naming you branches to your taste or even using
master directly if you find this appropriate. Still, keep in mind that:

- a git email or a pull request should address a single feature a bug,
  so keep your branches as tidy as possible;
- this is a small project with a limited number of forks and active threads
  on Github, so one might want to look at your fork and find the branch you
  are using to implement a specific feature; to avoid any hassle, we suggest
  that you use branch names prefixed with ``feat-`` or ``fix-`` and followed
  either by the name of the Github issue or a short and meaningful name.

Forked projects
---------------

If you find yourself forking the project for a specific independant purpose
(commercial use, different phylosophy or incompatible point of view), we would
be glad if you let us know so that we can list interesting known forks and
their specific features (and backport some of them if your implementation
is free as well).