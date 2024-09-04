# asynchronous & synchronous
### asynchronous
- slack, email, video recording(zoom), project management software(trello), wikis...

# ASGI before WSGI
WSGI is for synchronous code.

Do not freely mix synchronous and asynchronous code. 
Because synchronous code can block an event loop in asynchronous code.
you can't simply write an asynchronous callable and plug it in. 

So ASGI was born. It is a new standard similar to WSGI.
And ASGI is for asynchronous code.

ASGI == a superset of WSGI.
ASGI can call WSGI callables.
ASGI has support for long polling,
    slow streaming,
    other response types without side-loading resulting in faster responses.

long polling: a backend technique. to achieve a real-time connection between a cliend and a server.

There is asgi.py file alongside wsgi.py file.
Take care not to call any sync code in the ASGI Handler in your asgi.py.

Performance
- The ASGI performance is much more stable even after failures.
- tool: Docker includes Nginx and Postgresql, Locust

Channels 
- support asynchronous protocols.
- asynchronous protocols: Websockets, long polling HTTP
- a official Django project
- not part of core Django
- ex. a chat application over web sockets
- If you want to make an async function in a Django view, have to wait for Django's Async support for views

Async
- is a part of Django core

asgiref
- is installed when installing Django 3.0
- contains sync-to-async and async-to-sync function wrappers
- contains a StatelessServer
- contains a WSGI-to-ASGI adapter
- can call sync code from async and vice versa


[Reference]('https://arunrocks.com/a-guide-to-asgi-in-django-30-and-its-performance/')