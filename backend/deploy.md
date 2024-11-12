## Two ways to deploy a Django project
1. Directly on a server, docker
2. Serverless Cloud (Google Cloud Run / App Engine, AWS Elastic Beanstalk / ECS Fargate)

## The steps to deploy a Django project
1. Setup a proxy
2. Handle static/media files
3. Configuration

## Components that we're going to set to deploy
1. WSGI (Web Server Gateway Interface)
2. Persisntent Data with some way
3. Reverse Proxy

### Reverse Proxy
Accept the request into our application.

- Best practice when deploying Djnago. There's TWO ways.
1. WSGI server great at executing Python code.
    - It can serve static content (images, CSS and JavaScript).
    - But it's not super efficient at images, CSS and JavaScript.
2. Web servers
    - It's really efficient at images, CSS and JavaScript.

## Applications
- nginx: web server.
- uWSGI
- Docker Compose: using it to pull all together on our server.

## Docker Compose Setup
1. App service
    is used to run app with uWSGI
2. Postgres datbase
    is used to store the persistent data in the database
3. Proxy - NGINX
    add proxy or a reverse proxy. It's going to handle the requests
4. static-data volume
    to store all the static data media files
5. another volume
    to store the persistent data for the database

Store the data even our server is stoped.

### Handling configuring such as database password, Django secret
1. Create .env file on the server.
2. Set the values inside Docker Compose.

And AWS.

---

move to proxy_code.md