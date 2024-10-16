# Build Docker on a project
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

    ENV PATH="/py/bin:$PATH"

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
- flake8 install - requirements.dev.txt
    ```
    flake8>=3.9.2,<3.10
    ```
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
- .flake8 create in app folder and write
    ```
    [flake8]
    exclude =
        migrations,
        __pycache__,
        manage.py,
        settings.py
    ```
- run command in terminal
    ```
    docker compose build
    docker compose run --rm app sh -c "flake8"
    ```
- create Django project
- run command to start docker server the django project
    ```
    docker compose up
    ```

# Test on Docker
`docker compose run --rm app sh -c "python manage.py test"`

# Configurate Database
1. Dockerfile
    ```
    RUN python -m venv /py && \
    /py/bin/pip install --upgrade pip && \
    apk add --update --no-cache postgresql-client && \ # installed postgresql client(installed inside Alpine image, to be able to connect to Postgres)
    apk add --update --no-cache --virtual .tmp-build-deps \ # sets a virtual dependencies package. matches with below del line.
        build-base postgresql-dev musl-dev && \ # install packages
    /py/bin/pip install -r /tmp/requirements.txt && \
    if [ $DEV = "true" ]; \
        then /py/bin/pip install -r /tmp/requirements.dev.txt; \
    fi && \
    rm -rf /tmp && \
    apk del .tmp-build-deps && \
    adduser \
        --disabled-password \
        --no-create-home \
        django-user
    ```

    docker-compose: Check it on GitHub

2. requirements.txt
    ```
    psycopg2>=2.8.6,<2.9
    ```
3. settings.py
    ```
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'HOST': os.environ.get('DB_HOST'), # match up inside docker-compose.yml
            'NAME': os.environ.get('DB_NAME'),
            'USER': os.environ.get('DB_USER'),
            'PASSWORD': os.environ.get('DB_PASS'),
        }
    }
    ```
4. Handle race condition "wait for db"
    (1) Create a new app
    (2) Modify the structure

    |core(app)|||
    |---|---|---|
    |management|__init\__.py||
    ||commands|__init\__.py|
    |||wait_for_db.py|
    |tests|__init\__.py|
    ||test_commands.py|

    (3) Write temporally wait_for_db.py
    ```python
    """
    Django command to wait for the database to be available.
    """
    from django.core.management.base import BaseCommand

    class Command(BaseCommand):
        """Django command to wait for database."""

        def handle(self, *args, **options):
            pass
    ```

    (4) Write test_commands.py
    ```python
    @patch('core.management.commands.wait_for_db.Command.check')
    class CommandTests(SimpleTestCase):
        """Test commands."""

        def test_wait_for_db_read(self, patched_check):
            """Test waiting for database if database ready."""
            patched_check.return_value = True

            call_command('wait_for_db')

            patched_check.assert_called_once_with(databases=['default'])

        def test_wait_for_db_delay(self, patched_check):
            """Test waiting for database when getting OperationalError."""
            patched_check.side_effect = [Psycopg2Error] * 2 + \
                [OperationalError ] * 3 + [True] # write some exception.

            call_command('wait_for_db')

            self.assertEqual(patched_check.call_count, 6)
            patched_check.assert_called_with(datbases=['default'])
    ```
    `patch`: mocking
    `patched_check`: customize the behavior.
    `patched_check.return_value = True`: Return True when check is called
    `call_command('wait_for_db')`: DB ready? Is the command set up correctly?
    `patched_check.assert_called_once_with(database=['default'])`: Call 'check' method inside Command.

    `patched_check.side_effect`: I set `patched_check.return_value = True` above function but I set some exceptions in this function. It would be raise exception when the database wasn't ready. we call the mocked method 2 times and we want to raise Psycopg2Error. And next 3 times OperationalError.
    `self.assertEqual(patched_check.call_count, 6)`: call 'patched_check' till the true.
    `patched_check.assert_called_with(datbase=['default'])`: database is ready?

    (5) Modify wait_for_db.py
    This method returns True if the DB is ready.
    ```python
        def handle(self, *args, **options):
            """Entrypoint for command."""

            self.stdout.write('Waiting for database...')
            db_up = False
            while db_up is False:
                try:
                    self.check(databases=['default'])
                    db_up = True
                except (Psycopg2Error, OperationalError):
                    self.stdout.write('Database unavailable, waiting 1 second ...')
                    time.sleep(1)

            self.stdout.write(self.style.SUCCESS('Database available!'))
    ```
    `self.stdout.write('Waiting for database...')`: To do log message on screen.

    (6) Running tests
    ```bash
    docker compose run --rm app sh -c "python manage.py test"
    ```

    I had errors like below.
    ```bash
    ./core/tests/test_commands.py:18:52: W291 trailing whitespace
    ```

    I handled it through remove and add spaces.

    (7) git push and unit test on GitHub Action.
    I checked GitHub Action and it was successfully handled race condition.

