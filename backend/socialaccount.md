
```python
class UserAPIView(APIView):
    def get(self, request):
        print(request.data)
```

{
  '_request': <WSGIRequest: GET'/'>,
  'parsers': [
    <rest_framework.parsers.JSONParserobjectat0x102ab3400>,
    <rest_framework.parsers.FormParserobjectat0x102ab3ca0>,
    <rest_framework.parsers.MultiPartParserobjectat0x102ab34f0>
  ],
  'authenticators': [
    <rest_framework.authentication.SessionAuthenticationobjectat0x1>,
    <rest_framework.authentication.BasicAuthenticationobjectat0x1>
  ],
  'negotiator': <rest_framework.negotiation.DefaultContentNegotiationobjectat0x102ab3c10>,
  'parser_context': {
    'view': <users.views.UserAPIViewobjectat0x1>,
    'args': (),
    'kwargs': {
      
    },
    'request': <rest_framework.request.Request: GET'/'>,
    'encoding': 'utf-8'
  },
  '_data': <class'rest_framework.request.Empty'>,
  '_files': <class'rest_framework.request.Empty'>,
  '_full_data': <class'rest_framework.request.Empty'>,
  '_content_type': <class'rest_framework.request.Empty'>,
  '_stream': <class'rest_framework.request.Empty'>,
  'accepted_renderer': <rest_framework.renderers.TemplateHTMLRendererobjectat0x102ab31f0>,
  'accepted_media_type': 'text/html',
  'version': None,
  'versioning_scheme': None,
  'csrf_processing_done': True,
  '_authenticator': <rest_framework.authentication.SessionAuthenticationobjectat0x102ab3f10>,
  '_user': <SimpleLazyObject: <User: test@gmail.com>>,
  '_auth': None
}
