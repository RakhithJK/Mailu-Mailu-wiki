Generating migration scripts
============================

After you modify the database models, you need to generate a new schema
migration script:

```
python manage.py db migrate
```

This will generate a new script in ``migrations/versions`` that you must review
before committing it.

Updating information files
==========================

If you added a feature or fixed a bug or committed anything that is worth mentionning
for the next upgrade, add it in the ``CHANGELOG.md`` file.

Also, if you would like to be mentionned by name or add a comment in ``AUTHORS.md``,
feel free to do so.