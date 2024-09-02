# django.db.utils.OperationalError: no such column: cart.price_id
1. Delete file(s) in migrations(except __init__.py)
2. Drop the table
```python
import sqlite3
con = sqlite3.connect('db.sqlite3')
c = con.cursor()
c.execute('DROP TABLE tablename');
```
3. Migration
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

If you got an error message like below,
```bash
Running migrations:
  No migrations to apply.
```

1. The codes that `migrations.CreateModel` transfer to comments.
This code was on `appname/migrations/0001_initial.py`.

2. Try again migration
```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

# django.db.utils.IntegrityError: FOREIGN KEY constraint failed
Exception raised whenn the relational integrity of the database is affected.
`user = models.ForeignKey('users.User', on_delete=models.CASCADE)` means
if user was deleted this(cart) data will delete too.

### on_delete options
1. CASCADE
When the referenced object is deleted, all related objects are also deleted.

2. PROTECT
Prevents the referenced object from being deleted. For example, if you want to delete a 'Board' that includes 'Comments,' you must delete the Comments first, then the Board.

3. SET_NULL
This option sets the related field to NULL when the referenced object is deleted. It requires the ForeignKey field to have null=True.

4. SET_DEFAULT
Assigns a default value to the related field when the referenced object is deleted. For instance, if you delete a post, its comments will be assigned to a default post you set in the model.

5. DO_NOTHING
No action is taken when the referenced object is deleted.
