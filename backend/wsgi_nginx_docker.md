1. Create proxy folder
2. Create Dockerfile on proxy folder
3. Create default.conf.tpl on proxy folder
4. Create uwsgi_params on proxy folder
5. Create run.sh on proxy folder


# Add uWSGI to project
configure dockerfile

Proxy folder
1. default.conf.tpl
2. uwsgi_params
3. run.sh
4. Dockerfile

docker compose -f docker-compose-deploy.yml down
docker compose -f docker-compose-deploy.yml up
---

1. Create AWS account and user
2. Login to console
3. Create new virtual server

---
### Add uWSGI
1. Dockerfile
- script
- install linux headers
- excute script
- run command

```dockerfile
COPY ./scripts /scripts

RUN python -m venv /py && \
    ...
        build-base postgresql-dev musl-dev zlib zlib-dev linux-headers && \
    ...
    chmod -R +x /scripts

ENV PATH="/scripts:/py/bin:$PATH"

CMD ["run.sh"]
```
wsgi Python package requires Linux headers installed.

2. Create run.sh
on root, /scripts/run.sh
```bash
#!/bin/sh

set -e

python manage.py wait_for_db
python manage.py collectstatic --noinput
python manage.py migrate

uwsgi --socket :9000 --workers 4 --master --enable-threads --module app.wsgi
```

3. requirements.txt
```
uwsgi>=2.0.19,<2.1
```

```bash
docker compose build
```

### Create proxy
1. Create proxy folder on root
2. Create configuration file.
/proxy/default.conf.tpl
3. default.conf.tpl
```
server {
    listen ${LISTEN_PORT};

    location /static {
        alias /vol/static;
    }

    location / {
        uwsgi_pass           ${APP_HOST}:${APP_PORT};
        include              /etc/nginx/uwsgi_params;
        client_max_body_size 10M;
    }
}
```
4. configurate 'uwsgi_params'. Create /proxy/uwsgi_params
[Copy and Paste this page]('https://uwsgi-docs.readthedocs.io/en/latest/Nginx.html#what-is-the-uwsgi-params-file')


5. Create /proxy/run.sh
```bash
#!/bin/sh

set -e # To make sure the shell script fails any of the subsequent commands fail.

envsubst < /etc/nginx/default.conf.tpl > /etc/nginx/conf.d/default.conf
# we're piping in or we're inserting the force actually etc/nginx/default.conf.tpl which is going to be our default.conf.tpl file that we created that's going to be copied to that location in docker image.
nginx -g 'daemon off;' # start engine with above
```

Until kill the running nginx server, docker container is going to continue to run.

6. Create Dockerfile
It will run Nginx our project.
/proxy/Dockerfile

```dockerfile
FROM nginxinc/nginx-unprivileged:1-alpine : Base image
LABEL maintainer='gahyunson.com' : metadata of the image

COPY ./default.conf.tpl /etc/nginx/default.conf.tpl
COPY ./uwsgi_params /etc/nginx/uwsgi_params
COPY ./run.sh /run.sh

ENV LISTEN_PORT=8000
ENV APP_HOST=app
ENV APP_PORT=9000

USER root

RUN mkdir -p /vol/static && \
    chmod 755 /vol/static && \
    touch /etc/nginx/conf.d/default.conf && \
    chown nginx:nginx /etc/nginx/conf.d/default.conf && \
    chmod +x /run.sh

VOLUME /vol/static

USER nginx

CMD ["/run.sh"]
```
docker build .


### Handling configuring such as database password, Django secret
1. .env
```
DB_NAME=dbname
DB_USER=rootuser
DB_PASS=changeme
DJANGO_SECRET_KEY=changeme
DJANGO_ALLOWED_HOSTS=127.0.0.1
```

2. docker-compose-deploy.yml
```
services:
  app:
    build:
      context: .
    restart: always
    volumes:
      - static-data:/vol/web
    environment:
      - DB_HOST=db
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASS=${DB_PASS}
      - SECRET_KEY=${DJANGO_SECRET_KEY}
      - ALLOWED_HOSTS=${DJANGO_ALLOWED_HOSTS}
    depends_on:
      - db

  db:
    image: postgres:13-alpine
    restart: always
    volumes:
      - postgres-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=${DB_NAME}
      - POSTGRES_USER=${DB_USER}
      - POSTGRES_PASSWORD=${DB_PASS}

  proxy:
    build:
      context: ./proxy
    restart: always
    depends_on:
      - app
    ports:
      - 80:8000
    volumes:
      - static-data:/vol/static

volumes:
  postgres-data:
  static-data:

```

3. settings.py
```python
SECRET_KEY = os.environ.get('SECRET_KEY', 'changeme')

DEBUG = bool(int(os.environ.get('DEBUG', 0)))

ALLOWED_HOSTS = []
ALLOWED_HOSTS.extend(
    filter(
        None,
        os.environ.get('ALLOWED_HOSTS', '').split(','),
    )
)
```

4. docker-compose.yml
```
services:
  app:
    environment:
      ...
      - DEBUG=1
```

5. Test running it locally, just check everything works.
`docker compose docker-compose -f docker-compose-deploy.yml up`