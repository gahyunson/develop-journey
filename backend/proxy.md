# Proxy
- A server application
- An intermediary between a client requesting a resource and the server providing that resource
- Improves privacy, security, and possibly performance
- Instead of connecting directly to a server that can fultill a request for a resource(a file or web page)

### URI is different
Client directly request to server.
```bash
GET /favicon.ico HTTP/1.1
User-Agent: SuperBrowserv1.3
```
part of URI

Client request to proxy.
```bash
GET http://www.marys-antiques.com/index.html HTTP/1.0
User-Agent: SuperBrower v1.3
```
full URI (host name, port number)

> Web clients should never hit an application server directly.

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
/proxy/Dockerfile

```dockerfile
FROM nginxinc/nginx-unprivileged:1-alpine
LABEL maintainer='gahyunson.com'

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