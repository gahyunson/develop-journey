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

## nested serializers
- serializer inside a serializer
- Read only by default (can't create new object)
- Custom logic to make writable

```python
class TagSerializer(serializers.ModelSerializer):
    """Serializer for tags."""

    class Meta:
        model = Tag
        fields = ['id', 'name']
        read_only_fields = ['id']


class RecipeSerializer(serializers.ModelSerializer):
    """Serializer for recipe."""
    tags = TagSerializer(many=True, required=False)
```

```python
def create(self, validated_data):
    """Create a recipe."""
    tags = validated_data.pop('tags', [])
    recipe = Recipe.objects.create(**validated_data)
    auth_user = self.context['request'].user
    for tag in tags:
        tag_obj, created = Tag.objects.get_or_create(
            user=auth_user,
            # name=tag['name'],
            **tag, # all values
        )
        recipe.tags.add(tag_obj)

    return recipe
```
We want to take all of the values that are passed into the tag.

## Design
### Seperate serializers
We don't need to define all things in a single serializer.
Let's create separate serializers! Then, I wonder the reason that creating a separate serializers is better.
It is because when we upload data, we don't need any other values that aren't relevant to the upload process.
We separate APIs and the best practice is to only upload one type of data to an API.

Here is examples.
I'm creating a 'Photos' Model with features such as id, title, description and date.
Now, I want to add an image feature.

```python
class PhotoSerializer(serializers.ModelSerializer):
    class Meta:
        model = Photo
        fields = ['id', 'title', 'description', 'date']

class PhotoImageSerializer(serializers.ModelSerializer):
    class Meta:
        model = Photo
        fields = ['id', 'image']
        read_only_fields = ['id']
        extra_kwargs = {'image': {'required': True}}
```
We set the model to Photo and add the 'image' field. It's going to be more efficiently manage uploading data.