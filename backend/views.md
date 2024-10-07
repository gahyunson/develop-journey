# views
Handles a request that's made to a particular URL in Django.

## Provided by Django
`from rest_framework.authtoken.views import ObtainAuthToken`


```python
renderer_classes = api_settings.DEFAULT_RENDERER_CLASSES
```
- Default setting
- You wouldn't get a browable API that's used for Django framework. You can add it manually.

## APIView vs Viewsets
### APIView
- Focused around HTTP methods
- Useful for non CRUD APIs

### Viewsets
- Focused around actions: If you set the actions in a view, the action will perform. (retrieve, list, update, and destroy...) And it will map to HTTP methods.
- Map to Django models.
- Router: allow to automatically generate URLs. You don't need to specifically define the structure for the urls.
- Great for CRUD operations on models.

- the serializer of the parameter is the data already was validated.
    ```python
    def perform_create(self, serializer):
        serializer.save(user=self.request.user)
    ```