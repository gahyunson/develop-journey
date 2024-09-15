# Model
### Making queries
- Django automatically gives you a database-abstraction API
- A model class = A DB table
- If you need to add extra Manager method or to modify the initial QuerySet the Manager returns -> customize a Manager

### Override Primary key 
- Add below code into a mode argument.
`primary_key=True`

- Be careful not to choose field names that conflict with the models API (clean, save, or delete).

### blank
- blank is filtered at validation step.

### verbose_name
- the first positional argument (except for ForeignKey, ManyToManyField, OneToOneField)
`first_name = models.CharField("Person's first name", ...)`
- if ForeignKey, ManyToManyField, OneToOneField, use the keyword.
`models.ForeignKey(..., verbose_name="related place")`

### Multi-table inheritance

For example, 
```python
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

class Restaurant(Place):
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)
```
 Place Table Structure

| Field   | Type            | Null | Key     | Default | Extra          |
|---------|-----------------|------|---------|---------|----------------|
| id      | INTEGER PRIMARY KEY AUTOINCREMENT | NO   | PRIMARY | None    | Auto Increment |
| name    | VARCHAR(50)     | NO   |         | None    |                |
| address | VARCHAR(80)     | NO   |         | None    |                |


Restaurant Table Structure

| Field           | Type         | Null | Key     | Default | Extra          |
|-----------------|--------------|------|---------|---------|----------------|
| id              | INTEGER      | NO   | PRIMARY | None    |                |
| name            | VARCHAR(50)  | NO   |         | None    |                |
| address         | VARCHAR(80)  | NO   |         | None    |                |
| serves_hot_dogs | BOOLEAN      | NO   |         | False   |                |
| serves_pizza    | BOOLEAN      | NO   |         | False   |                |

### Proxy Model
It doesn't get its own database table operates on the original table.

How to set proxy model?
Add `proxy = True` attribute of the Meta class.

`Meta.maanged=False` If you are mirroring an existing model/database table and don't want all theoriginal database table columns.

`Meta.proxy=True` To change the Python-only behavior of a model, but keep all the same fields as in the original.



### Manager
Default managers
- use default manager or base_manager if you handle an unknown model(in a third-party app)


Custom Manager
- extending the base Manager class
- Instantiating your custom Manager in your model
- can return anything
- can access `self.model`

Modifying a manager's initial QuerySet

```python
class CartManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(user='gahyun')

class Photo(models.Model):
    ...
    cart_objects = CartManager()
```
With this model, `Photo.cart_objects.all()` will only return `.filter(user='gahyun')`


Accessing related objects
- Django uses an instance of the Model._base_manager (not the _default_manager)
- or Meta.bas_manager_name


