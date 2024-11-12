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
