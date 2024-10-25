# HTTP & content_length

Content_length makes a clear sign between client and server. If you are developing some HTTP methods, pay attention it.

The HTTP methods mean message body included. POST, PUT methods when request (from frontend) and GET methods when response (from backend).

content_length is part of the HTTP header.
The length of the message body, in bytes.
It can be used by the client to verify that the entire message has been received.

If you want to manually pass the Content_length header to the server, you need to add two things to your request.
1. Content-Length
2. Content-Type
which describes the size and type of data in the body of the POST request.

The data length must be specified in bytes.
For example, if you want to send below data,
```JSON
{"widget": {
    "window": {
        "title": "Sample Widget",
        "name": "sample_widget",
        "width": 600,
        "height": 400
    },
    "image": {
        "src": "images/test.png",
        "name": "sample_test",
        "hOffset": 150,
        "vOffset": 150,
        "alignment": "center"
    },
    "text": {
        "data": "Click Here",
        "size": 36,
        "style": "bold",
        "name": "sample_click",
        "alignment": "center"
    }
}}
```
This characters length is 473 so you'll write 'Content-Length: 473'.

```bash
POST /echo/post/json HTTP/1.1
Host: reqbin.com
Content-Type: application/json
Content-Length: 473
```


# Should I manually set Content-Length?
No you don't have to. Django automatically provide Content-Length.
Javascript too.

Try below codes, you can print Content-Length.
1. Javascript POST/PUT method request content_length: you can check here -> Django(Backend) views.py
```python
def post(self, request, *args, **kwargs):
        print(request.headers)
```

You'll get message like this.
```bash
{'Content-Length': '117',
'Content-Type': 'application/x-www-form-urlencoded',
'Host': '127.0.0.1:8000',
'Connection': 'keep-alive',
'Cache-Control': 'max-age=0',
'Sec-Ch-Ua': '"Chrom", "Not;", "Goog', 'Sec-': '?0', 'Sem': '"macOS"', 'Upgrsts': '1', 'Origin': 'http', 'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.0.0 Safari/537.36', 'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7', 'Sec-Fetch-Site': 'same-origin', 'Sec-Fetch-Mode': 'navigate', 'Sec-Fetch-User': '?1', 'Sec-Fetch-Dest': 'document', 'Referer': 'http://127.0.0.1:8000/api/photos/3/', 'Accept-Encoding': 'gzip, deflate, br, zstd', 'Accept-Language': 'ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7', 'Cookie': 'messages=.eJyLBNEki; sessionid=h3jtk484ah'}
```
You can check 'Content-Length'.

2. Django GET method response content_legnth: you can check here -> Django(Backend) make custom middleware, custom_middleware.py

1) Create custom_middleware.py
```python
class CustomMiddleware(CommonMiddleware):
    def process_response(self, request, response):
        response = super().process_response(request, response)

        content_length = response.headers['Content-Length']
        print('response content_length is ',content_length)
        return response
```

2) Add it in setting.py
```
MIDDLEWARE = [
    'appname.custom_middleware.CustomMiddleware',
]
```

3) Execute the GET method

4) Then you'll see below
response content_length is  9357

### plus. I noticed the number of execute log,
The log was '[30/Aug/2024 11:12:16] "GET /api/photos/2/ HTTP/1.1" 200 9357'
There were same number the end of the log.

I searched for it, and this means...

- [30/Aug/2024 11:12:16]: When log was written?
- GET: Kind of method.
- /api/photos/2/: URL path was requested a client.
- HTTP/1.1: HTTP protocol version.
- 200: successful request.
- 9357: The bytes size of the response's body from server to client.

Yes, Content-Length is the size of the response's body in bytes.
We always watched that ...

[Reference]('https://www.amazon.co.uk/HTTP-Definitive-Guide-David-Gourley/dp/1565925092')