# Cache Flow
You can see the flow of a cache mechanism in the diagram below.
I simply described how cache works.

1. request
2. Is it cached?
3. Is the cached data fresh?
And It's going to some process
4. deliver it to the client.

You can refer to the logics below.

| Question | YES | NO |
| ------- | --- | ---|
| request | 
| Is it cached | Is the cached data fresh? | Request to the origin server |
| Is the cached data fresh? | Deliver it to the client | Revalidate with server |
| Was the revalidation passed? | Renew the cache's freshness | Request to the origin server |

I know it's a little hard to reading, but you can search for 'cache flow' for more information.

### Is the Cache fresh?
The `Expires` HTTP header contains the expiration date/time of the cached data.

- Cache-Control: no-store    
    Any caches of any kind should not store response.

- Cache-Control: no-cache    
    Request directive asks caches to validate the response with the origin server before reuse.    

- Cache-Control: max-age    
    The site wants to serve fresh data unil 'max-age' seconds after the response is generated.

### How to set Cache handling?
1. Apache
2. HTTP-EQUIV
3. HTTP Headers

In my case, I tried 3rd way by setting cache control headers.
```python
class Main(APIView):
    @method_decorator(cache_control(max_age=604800))
    def get(self, request):
        return render(request, 'main.html')
```

Then, I checked the result in the terminal.
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

### Comparing the Time Before and After Caching
This is my current project, the 'hyunphoto' website.
I profiled it using django_toolbar.
And caching reduced the time to one-third of the original.

### <Before Caching\>

|Call|	CumTime|	Per|	TotTime|	Per|	Count|
|---|---|---|---|---|---|
|/debug_toolbar/panels/__init__.py in process_request(195)	|0.670|	0.670|	0.000|	0.000|	1|
|/asgiref/sync.py in __call__(164)	|0.670	|0.670	|0.000|	0.000	|1|
|/asgiref/current_thread_executor.py in run_until_future(61)|	0.670	|0.670	|0.000|	0.000	|1|
|/asgiref/current_thread_executor.py in run(35)|	0.668	|0.223	|0.000	|0.000	|3|
|/asgiref/sync.py in thread_handler(501)	|0.668	|0.223|	0.000	|0.000|	3|
|{method 'run' of 'Context' objects}|	0.668|	0.223|	0.000|	0.000	|3|
|/django/views/decorators/csrf.py in wrapper_view(54)|	0.668	|0.668|	0.000	|0.000	|1|
|/django/views/generic/base.py in view(96)	|0.668	|0.668|	0.000|	0.000	|1|
| /rest_framework/views.py in dispatch(485)	|0.667	|0.667	|0.000|	0.000|	1|
| /rest_framework/views.py in initial(399)	|0.638	|0.638	|0.000|	0.000|	1|
|/rest_framework/views.py in perform_authentication(316)	|0.636|	0.636|	0.000|	0.000|	1|
|/hyunphoto/hyunphoto/photos/views.py in get(19)	|0.029|	0.029|	0.000|	0.000|	1|

### <After Caching\>

|Call|	CumTime|	Per|	TotTime|	Per|	Count|
|---|---|---|---|---|---|
|/debug_toolbar/panels/__init__.py in process_request(195)|0.019|	0.019	|0.000|	0.000|	1|
| /asgiref/sync.py in __call__(164) |0.019	|0.019	|0.000	|0.000	|1|
| /asgiref/current_thread_executor.py in run_until_future(61) |0.018	|0.018|	0.000	|0.000|	1|
| /asgiref/current_thread_executor.py in run(35) |0.016|	0.005|	0.000	|0.000|	3|
|  /asgiref/sync.py in thread_handler(501) | 0.016|	0.005	|0.000	|0.000|	3|
|  {method 'run' of 'Context' objects} | 0.016	|0.005|	0.000|	0.000|	3|
|  /django/views/decorators/csrf.py in wrapper_view(54) | 0.016	|0.016	|0.000	|0.000|	1|
|  /django/views/generic/base.py in view(96) | 0.016|	0.016	|0.000|	0.000|	1|
|  /rest_framework/views.py in dispatch(485) | 0.016|	0.016|	0.000	|0.000|	1|
|  /rest_framework/views.py in initial(399) | 0.010	|0.010|	0.000|	0.000|	1|
| /rest_framework/views.py in perform_authentication(316) | 0.009|	0.009	|0.000|	0.000|	1|
|  /hyunphoto/hyunphoto/photos/views.py in get(19) | 0.006|	0.006	|0.000|	0.000|	1|
| /django/shortcuts.py in render(17) | 0.006	|0.006	|0.000	|0.000	|1|