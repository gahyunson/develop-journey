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