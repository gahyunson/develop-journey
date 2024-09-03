# request flow

1. request
2. web server
3. 
    1) static -> Filesystem
    2) dynamic -> WSGI Server
4. Middleware
5. Django App
    1) URL Conf
    2) Views
    3) Models -(software do nothing this moment)-> 4. DB    
    (or) Templates

# ASGI before WSGI
WSGI is for synchronous code.

Do not freely mix synchronous and asynchronous code. 
Because synchronous code can block an event loop in asynchronous code.
you can't simply write an asynchronous callable and plug it in. 

So ASGI was born. It is a new standard similar to WSGI.
And ASGI is for asynchronous code.


