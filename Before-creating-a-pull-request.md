Generate migration scripts
==========================

After you modify the database models, you need to generate a new schema
migration script:

```
python manage.py db migrate
```

This will generate a new script in ``migrations/versions`` that you must review
before committing it.

Update translations
===================

Freeposte.io uses Babel for internationalization and localization. Before any
of your work is merged, you must make sure that your strings are internationalized
using Babel.

If you used ``_``, ``{% trans %}`` and other Babel syntaxes in your code, run the
following command to update the POT file:

```
pybabel extract -F babel.cfg -k lazy_gettext -o messages.pot freeposte
```

The, update the translations:

```
pybabel update -i messages.pot -d translations
```

Please resolve fuzzy strings to the best of your knowledge.

Update information files
========================

If you added a feature or fixed a bug or committed anything that is worth mentionning
for the next upgrade, add it in the ``CHANGELOG.md`` file.

Also, if you would like to be mentionned by name or add a comment in ``AUTHORS.md``,
feel free to do so.