# Configure a project to use static files

1. Create directories that can be used to store media files.
Dockerfile
```dockerfile
RUN python -m venv /py && \
    ...
    adduser \
        ...
        django-user && \
    mkdir -p /vol/web/media && \
    mkdir -p /vol/web/static && \
    chown -R django-user:django-user /vol && \
    chmod -R 755 /vol
```

2. docker compose build
3. docker-compose.yml
```yml
services:
  app:
    volumes:
      - dev-static-data:/vol/web

volumes:
  dev-static-data:
```

4. setting.py
```Python
STATIC_URL = '/static/static/'
MEDIA_URL = '/static/media/'

MEDIA_ROOT = '/vol/web/media'
STATIC_ROOT = '/vol/web/static'
```

5. project/urls.py
```python
from django.conf.urls.static import static
from django.conf import settings

if settings.DEBUG:
    urlpatterns += static(
        settings.MEDIA_URL,
        document_root=settings.MEDIA_ROOT,
    )
```

