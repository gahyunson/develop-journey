Enable to documentation with `drf-spectacular`

1. requirements.txt

Install drf-spectacular to the project.
```
drf-spectacular>=0.15.1,<0.16
```

2. settings.py

Set up the configuration.
```python
INSTALLED_APPS = [
    'rest_framework',
    'drf_spectacular',
]

REST_FRAMEWORK = {
    'DEFAULT_SCHEMA_CLASS': 'drf_spectacular.openapi.AutoSchema',
}
```

3. project/urls.py

Serve a documentation through the Django project.

```python
from drf_spectacular.views import (
    SpectacularAPIView,
    SpectacularSwaggerView,
)

urlpatterns = [
    # schema for api , scribe the api
    path('api/schema/', SpectacularAPIView.as_view(), name='api-schema'),
    # serve the swagger documenataion
    path(
        'api/docs/',
        SpectacularSwaggerView.as_view(url_name='api-schema'),
        name='api-docs',
    ),
]
```
