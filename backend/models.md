Making queries
- Django automatically gives you a database-abstraction API
- A model class = A DB table
- If you need to add extra Manager method or to modify the initial QuerySet the Manager returns -> customize a Manager

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
