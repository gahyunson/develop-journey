# Private Methods
- It should neither be accessed outside the class nor by any base class.
- To define a private method prefix the member name with the double underscore __.
- __methodname

```python
class Base:
    # public method
    def publicmethod(self):
        return "Public method"

    # private method
    def __privatemethod(self):
        return "Private method"
```