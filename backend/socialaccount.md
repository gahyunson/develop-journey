
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



저장된 패스워드 값이 보안에 신경써야하는 값인가?
Do I have to care 'password' was stored in DB?

I took a look allauth github source.

1. `allauth/socialaccount/urls.py`
```python
path("signup/", views.signup, name="socialaccount_signup"),
```
I'm going to signup on view.py.

2. `allauth/socialaccount/views.py`
```python
signup = SignupView.as_view()
```
and let's go to SignupView

```python
class SignupView(...):
    form_class = SignupForm
```
I couldn't guess the role of the functions in SignupView, But I could guess that there's something what I need in SignupForm.

3. `allauth/socialaccount/forms.py`
I could guess something happen in `get_adapter()` before user saved.
```python
class SignupForm(BaseSignupForm):
    def save(self, request):
        adapter = get_adapter()
        user = adapter.save_user(request, self.sociallogin, form=self)
        self.custom_signup(request, user)
        return user
```

4. `allauth/socialaccount/adapter.py`
I was looking around codes in
```python
class DefaultSocialAccountAdapter(BaseAdapter):
```

And found something suspicious.

```python
def save_user(self, request, sociallogin, form=None):
        """
        Saves a newly signed up social login. In case of auto-signup,
        the signup form is not available.
        """
        u = sociallogin.user
        u.set_unusable_password()
        if form:
            get_account_adapter().save_user(request, u, form)
        else:
            get_account_adapter().populate_username(request, u)
        sociallogin.save(request)
        return u
```
I noticed 'u.set_unusable_password()' and checked the password before unusabled.

I experiment two case and before password is not equal with after password.
```python
u = sociallogin.user
print('before password:', u.password)
u.set_unusable_password()
print('after password:', u.password)
```

5. set_unusable_password()
If you set password with set_unusable_password(), 
Login is impossible with password, possible only with OAuth.
u.set_unusable_password() is different with Blank or None value.
