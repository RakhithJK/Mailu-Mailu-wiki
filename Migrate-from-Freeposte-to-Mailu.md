Freeposte and Mailu actually are the same mail server distribution. In the process of renaming the project however, we introduced a few changes that may break your server if you do not pay enough attention. Here is a series of changes you should apply before you pull the Mailu images.

Read the Changelog
==================

First supported Mailu release is ``1.3`` and has no counterpart on Freeposte.io. Thus, migrating will require that you upgrade to ``1.3`` (either by staying on the stable branch or by switching to ``1.3`` manually).

Make sure that you read the changelog first!

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

About the tracker
=================

We are very strict on the no-tracking policy and make sure that none of the pieces we put together includes a tracker that could endanger our users privacy. So why include one in Freeposte after we renamed the project?

The tracker is actually included in a notification bar displayed on top of the administration interface that tells administrators to move to Mailu. It is only included for users that upgraded their Freeposte installation (ran a ``docker-compose pull``) after the project was renamed. Thus it will only leak information from Freeposte administrators browsers during the short time between the upgrade and the migration. 

And to the why: we needed a simple metric of how many users were still running and upgrading the old Freeposte distribution before we shut it down for good. On top of this, we do not store any other information than the fact that a hit was recorded on the tracker image, neither your IP address nor your user agent is stored and logs will be flushed after we shut down Freeposte.
