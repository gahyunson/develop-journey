### helper function
We can use some helper function in tests.py.
For example,
```python
def create_user(**params):
    return get_user_model().objects.create_user(**params)

def test_update(self):
    new_user = create_user(email='user@example.com', password='test123')
    ...
```

If you don't use helper function,
```python
def test_update(self):
    self.user = get_user_model().objects.create_user(
        'user@example.com',
        'testpass123',
    )
```

### API method
Particial update.
```python
payload = {'title': 'New recipe title'}
url = detail_url(recipe.id)
res = self.client.patch(url, payload)
```

### Refresh Database
Django doesn't automatically refresh from DB.
It will refresth from DB.
```python
recipe.refresh_from_db()
```