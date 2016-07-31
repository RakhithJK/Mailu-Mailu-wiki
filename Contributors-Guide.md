Development environment
=======================

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

Philosophy
==========

The mailserver is designed as a whole, some images are therefore not best
suited for reuse outside this project. All images should however follow
Docker best practices and be as generic as possible :

 - even if not suited for reuse, they should be simple enough to
   fit as base images for other projects,
 - interesting settings should be available as environment variables
 - base images should be well-trusted (officiel Alpine or Debian for instance).
