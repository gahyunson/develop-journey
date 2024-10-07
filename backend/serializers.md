# Serializer
- Convert input data into model objects.

### serializers.ModelSerializer
- Module serializer
- Allow to automatically validate and save things to a specific model.

### serializers.Serializer
- It isn't linked to a specific model.

### CustomSerializer
CustomDetailSerializer is an extension of CustomSerializer.
```python
class CustomDetailSerializer(CustomSerializer):
    """Serializer for recipe detail view."""

    class Meta(CustomSerializer.Meta):
        fields = CustomSerializer.Meta.fields + ['add_field']
```
Just add some fields.