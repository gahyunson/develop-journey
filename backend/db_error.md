# django.db.utils.OperationalError: no such column: cart.price_id
1. Delete file(s) in migrations(except __init__.py)
2. Drop the table
```python
import sqlite3
con = sqlite3.connect('db.sqlite3')
c = con.cursor()
c.execute('DROP TABLE tablename');
```