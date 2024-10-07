# Serializer
- Convert input data into model objects.

### serializers.ModelSerializer
- Module serializer
- Allow to automatically validate and save things to a specific model.
- Convert input data to model objects.

### serializers.Serializer
- Default serializer
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

## Override
If you need Override something,

### argument
- trim_whitespace=True/False: Automatically ignore a space/User may deliberately have a space