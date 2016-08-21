Docker containers
=================

The development environment is quite similar to the production one. You should always use
the ``testing`` version when developping. Simply uncomment the ``build`` directive on
containers that you are working on and run:

```
docker-compose build
```

whenever you want to re-build them.

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