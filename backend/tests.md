# Tests
1. Public tests (Unauthenticated requests)
    - e.g. registering a new user
2. Private tests (Anthenticated requests)


### full URL path
`from django.urls import reverse`
`reverse('user:create')`: URL endpoint of the API.
It returns full URL path.


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

### Create API and testing
```python
from rest_framework.test import APIClient

class PublicApiTests(TestCase):
    def setUp(self):
        self.client = APIClient()

    def test_create_user_success(self):
        """Test creating a user is successful."""
        payload = {
            'email': 'test@example.com',
            'password': 'testpass123',
            'name': 'Test Name',
        }
        res = self.client.post(CREATE_USER_URL, payload)
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

### Check the values
`self.assertIn('token', res.data)`: To check the res data includes a token.