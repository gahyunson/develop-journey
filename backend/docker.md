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

### Docker Compose
- Defined with project (re-usable)
- Persistent data using volumes
- Handles network configuration
- Environment variable configuration

Before Django connect to databse ... set in settings.py
- Engine (which is type of database)
- Hostname (which is IP or domain name for database, address our django connect to)
- Port (default:5432)
- Database Name
- Username (in order to authentication)
- Password

### Database configuration with Django

"wait for db" Fixing database race condition.
(Django is ready but DB isn't ready)
- Check for database availability
- Continue when database ready
-> Create custom Django manage command


```python
from unittest.mock import patch

from psycopg2 import OperationalError as Psycopg2Error

from django.core.management import call_command
from django.db.utils import OperationalError
from django.test import SimpleTestCase # unit test

@patch('core.management.commands.wait_for_db.Command.check')
class CommandTests(SimpleTestCase):
    """Test commands."""

    def test_wait_for_db_read(self, patched_check):
        """Test waiting for database if database ready."""
        patched_check.return_value = True
```