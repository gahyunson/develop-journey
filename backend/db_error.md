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

# django.db.migrations.exceptions.InconsistentMigrationHistory
```bash
django.db.migrations.exceptions.InconsistentMigrationHistory: Migration admin.0001_initial is applied before its dependency core.0001_initial on database 'default'.
```

```bash
docker volume ls
DRIVER    VOLUME NAME
local     01d6d6ac...
local     4eef9725...
local     6bbf9381...
local     8c4a1af0...
local     22b2cc2e...
local     39e7e528...
local     51aa3a34...
local     64eb4de1...
local     990b5e74...
local     a6a4ee69...
local     b3a542e4...
local     dd9a640e...
local     e25de1b2...
local     f0da9d4a...
local     fbc765ea...
local     recipe-app-api_dev-db-data
apple@MacBook-Pro recipe-app-api % docker volume rm recipe-app-api_dev-db-data
Error response from daemon: remove recipe-app-api_dev-db-data: volume is in use - [7195f5cc84260092d784aff8beb26fd6000b89022d682d2ca90fc84c6bac16fa]
apple@MacBook-Pro recipe-app-api % docker compose down
[+] Running 3/3
 ✔ Container recipe-app-api-app-1  Removed                                                               0.0s
 ✔ Container recipe-app-api-db-1   Removed                                                               0.4s
 ✔ Network recipe-app-api_default  Removed                                                               0.1s
apple@MacBook-Pro recipe-app-api % docker volume rm recipe-app-api_dev-db-data
recipe-app-api_dev-db-data
apple@MacBook-Pro recipe-app-api % docker volume ls
DRIVER    VOLUME NAME
local     01d6d6ac...
local     4eef9725...
local     6bbf9381...
local     8c4a1af0...
local     22b2cc2e...
local     39e7e528...
local     51aa3a34...
local     64eb4de1...
local     990b5e74...
local     a6a4ee69...
local     b3a542e4...
local     dd9a640e...
local     e25de1b2...
local     f0da9d4a...
local     fbc765ea...
```