# Cache Flow
You can see the question's node.
I simply write the flow of cache.

1. request
2. was it caching?
3. Is it fresh?
And It's going to some process then finally
4. deliver to the client.

You can refer the logics below.

| Question | YES | NO |
| ------- | --- | ---|
| request | 
| Was it caching? | Is it fresh? | request to the origin server |
| Is it fresh? | deliver to the client | revalidate with server |
| Was the revalidation passed? | renew the cache's freshness | request to the origin server |

I know it's a little hard to reading. or you can search with the keyword 'cache flow'.

### Is Cache fresh?
The `Expires` HTTP header contains the date/time.

Cache-Control: no-store   
    Any caches of any kind should not store response.

Cache-Control: no-cache
    Request directive asks caches to validate the response with the origin server before reuse.    

Cache-Control: max-age
    The site wants to serve fresh data unil 'max-age' seconds after the response is generated.

### How to set Cache handling?
1. Apache
2. HTTP-EQUIV
3. HTTP Header

I tried 3rd way.
```python
class Main(APIView):
    @method_decorator(cache_control(max_age=604800))
    def get(self, request):
        return render(request, 'main.html')
```

And check on terminal.
```bash
curl -I http://127.0.0.1:8000/
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Cache-Control: max-age=604800
Vary: Accept, Cookie
Allow: GET, HEAD, OPTIONS
X-Frame-Options: DENY
Content-Length: 4608
X-Content-Type-Options: nosniff
Referrer-Policy: same-origin
Cross-Origin-Opener-Policy: same-origin
Server: daphne
```