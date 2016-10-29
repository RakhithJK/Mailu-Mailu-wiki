Freeposte and Mailu actually are the same mail server distribution. In the process of renaming the project however, we introduced a few changes that may break your server if you do not pay enough attention. Here is a series of changes you should apply before you pull the Mailu images.

Stop Freeposte
==============

Before anything else, stop Freeposte to avoid modifying live files. You should also delete the containers to make sure you re-create them afterwards,

```
docker-compose down
```

Use the latest ``docker-compose.yml``
=====================================

The latest Docker Compose configuration file is available from this Git repository. It points to the new ``mailu/`` and images and should replace yours.

If you applied some changes to your Docker Compose configuration file, make sure that you merge them with ours.

Use the latest ``.env``
=======================

The latest environment file is available from this Git repository. It has the new Docker Compose project name. Make sure that you merge your configuration changes with our changes.


Move data to the proper path
============================

If you did not change the default path for storing Freeposte/Mailu data, please make sure that you either set the ``ROOT`` variable properly, or move your data to the proper path:

```
mv /freeposte /mailu
```

Then, inside your root directory, move the following files and directories:

```
mv freeposte data
mv data/freeposte.db data/main.db
```

Start Mailu
===========

Finally, you should be ready to start Mailu:

```
docker-compose up -d
```