# I want to know the values of the parameters!
These are automatically provided by Django 
```python
def post(self, request, *args, **kwargs):
    print(self.__dict__ )
```

```json
{
  'head': <boundmethodPhotoDetailView.getof<photos.views.PhotoDetailViewobjectat0x109204760>>,
  'request': <rest_framework.request.Request: POST'/api/photos/2/'>,
  'args': (),
  'kwargs': {
    'photo_id': 2
  },
  '_negotiator': <rest_framework.negotiation.DefaultContentNegotiationobjectat0x10923bdf0>,
  'headers': {
    'Allow': 'GET, POST, HEAD, OPTIONS',
    'Vary': 'Accept'
  },
  'format_kwarg': None
}
```

### 1. self
- Represents the instance of the class.
- Provides access to instance attributes and methods.
- Used to maintain and access the state of the object.

### 2. request
- An object encapsulating HTTP request information.
- Contains data such as headers, body, method, and user authentication details.
- Crucial for accessing and processing client-submitted data.

    It is automatically provided Parser by Django.   
    You can use it when you need forms JSON, XML, HTML etc. in RESTful API.

    'request.data' allows you to access the body of the request in a structured format without needing to manually parse it.

### 3. *args
- Collects additional positional arguments.
- Allows for flexible method signatures.
- Useful when the number of arguments is not known in advance.

### 4. **kwargs
- Captures additional keyword arguments as a dictionary.
- Often contains URL parameters in Django views.
- Enables dynamic parameter handling and enhances method flexibility.
- For example, if you have a URL pattern that includes a parameter like '<int:photo_id>' this value would be passed in **kwargs.

### *args AND **kwargs
*args is
- tuple type (value , )
- Non-Keyword Arguments
- pass a variable number of arguments to a function

**kwargs is
- dictionary type {key: value}
- Keyword Argument

Keep always a sequence '*args', '**kwargs'.
