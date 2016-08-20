Generating migration scripts
============================

After you modify the database models, you need to generate a new schema
migration script:

```
python manage.py db migrate
```

This will generate a new script in ``migrations/versions`` that you must review
before committing it.