I'm new at Docker. I only know docker is something about container.
Now I know the best basic tutorial is at official site.
So I'd opened Django application and followed the tutorial.

Everything was great but I confronted a path problem. So I'm going to explore Docker.

# Why Docker?
- A consistent development (I'm using exactly the same code) Production environment
- Easier collaboration
- I don't need to install things manually
- I can also add all of the operating system level dependencies (in my Docker file)
- I can simply delete the projects files and delete all of the Docker images

# How use?
1. Define Dockerfile (contains all of the operating system level dependencies that the project needs)
2. Create Docker Compose configuration (contains how to run the images that are created from the Dockerfile configuration)
3. Run all commands via Docker Compose

# Where store docker token?
1. Create docker token in my account of docker hub
2. Paste the token code in secrets of github repository

## docker-compose.yml
version: the version of docker compose syntax that we're going to be using.   
services: main block. - docker-compose file is typically consist of one or more services as need it for my application.   
app: name about service, run on docker file.   
build:   
  context: root (current directory) -> 이 부분 다시 들어봐야 함   
  args:
ports:   
volumes: mapping directly from our system into the docker container, automatically sink the code and our project to the running container.  
command: 

Linting: check code formatting
    install flake8
    Run it through Docker Compose


- .dockerignore create and write
- requirements.txt create and write
    ```
    Django>=3.2.4,<3.3
    djangorestframework>=3.12.4,<3.13
    ```
- Create 'app' folder on root dir
- Dockerfile create and write 
    ```
    FROM python:3.9-alpine3.13
    LABEL maintainer="aaa.com"

    ENV PYTHONUNBUFFERED 1 # recommended running python in docker container.

    COPY ./requirements.txt /tmp/requirements.txt  
    COPY ./app /app  
    WORKDIR /app
    EXPOSE 8000

    ARG DEV=false
    RUN python -m venv /py && \
        /py/bin/pip install --upgrade pip && \
        /py/bin/pip install -r /tmp/requirements.txt && \
        adduser \
            --disabled-password \
            --no-create-home \
            django-user 

    ENV PATH='/py/bin:$PATH'

    USER django-user
    ```
- docker-compose.yml create and write
    ```
    # version: "3.9"
    services:
    app:
        build:
            context: .
        ports:
            - "8000:8000"
        volumes:
            - ./app:/app 
        command: >
            sh -c "python manage.py runserver 0.0.0.0:8000"
    ```
- flake8 install
- Add code to Dockerfile
    ```
    RUN ...
    if [ $DEV = "true" ]; \ 
        then /py/bin/pip install -r /tmp/requirements.dev.txt; \
    fi && \
    rm -rf /tmp && \
    ```
- Add code to docker-compose.yml
    ```
    build:
      context: .
      args:
        - DEV=true
    ```
