# Returning URLs

## reverse
`reverse(viewname, *args, **kwargs)`
It returns a fully qualified URL.

1. Generate URL, using its ID. in test.py
```python
def detail_url(something_id):
    """Create and return an detail URL."""
    return reverse('someset:something-detail', args=[something_id])
```

2. Add `UpdateModelMixin` on view class to update.
```python
class IngredientViewSet(mixins.UpdateModelMixin,
                        blah blah ... ):
```

3. Use the function generates the URL.
```python
def test_blah_blah(self):
    url = detail_url(ingredient.id)
    ...
```

Django works a lot of things for me. Sometimes it's too hard understand soon because it skipped a lot of steps.