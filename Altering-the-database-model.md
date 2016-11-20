Alter the model code
====================

First alter the model code. Simply edit ``models.py`` and modify all required model classes.
Make sure that your changes reflect on related classes as the migration should be consistent from one structure to another.

Do not commit your changes at that stage.

Generate migration scripts
==========================

After you modify the database models, you need to generate a new schema
migration script:

```
python manage.py db migrate
```

This will generate a new script in ``migrations/versions`` that you must review
before adding it for commit.

Because we use SQLite, make sure that you use batch operation for column changes and deletion. For instance:

```
def downgrade():
    with op.batch_alter_table('fetch') as batch:
        batch.drop_column('last_check')
        batch.drop_column('error')
```

or

```
def upgrade():
    # spam_threshold is a X/15 based value, we're converting it to percent.
    for user in User.query.all():
         user.spam_threshold = int(100. * float(user.spam_threshold or 0.) / 15.)
    db.session.commit()

    # set default to 80%
    with op.batch_alter_table('user') as batch:
        batch.alter_column('spam_threshold', default=80.)
```

Upgrade your local database
===========================

At that point, to start working on the changed database structure, you will need to upgrade your local database. Make a backup of your database, then:

```
python manage.py db upgrade
```

If any error arises, restore the backup, fix the migration script and try again.
