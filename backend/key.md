# How to Retrieve Related Data Using a Reverse ForeignKey in Django
- When you have a ForeignKey relationship between two models, you can easily access related data using the reverse relation. This is especially useful when you want to retrieve related objects from the other side of the relationship.
- lowertablename_set.all()

example.
```python
class Photo(models.Model):

class Price(models.Model):
    photo = models.ForeignKey(Photo, on_delete=models.CASCADE)
```
If you want to get price data on Photo model,
```python
photo = Photo.objects.get(id=1)  # Retrieve a specific Photo object
prices = photo.price_set.all()   # Get all related Price objects
```

and you can show it on template too.
```html
{% for price in photo.price_set.all %}
        <option value="{{ price.price }}" data-size="{{ price.size }}">{{ price.size }}</option>
{% endfor %}
```