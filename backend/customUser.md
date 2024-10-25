# Custom User model
You can override the default *user model* by using AbstractUser.
Simply import `AbstractUser`:

```python
from django.contrib.auth.models import AbstractUser

class User(AbstractUser):
    pass
```

If you want to use specific fields instead of all fields, you can define them in the serializer.
```python
class Meta:
        model = User
        fields = ('username', 'password', 'email')
        extra_kwargs = {'password': {'write_only': True}}
```

### get_user_model() Provides Flexibility
There are two ways to reference custom user model.
1. `CustomUser` directly. If the AUTH_USER_MODEL setting has been changed to `ChangeUser`, you have to change all the `CustomUser` to `ChangeUser`.
2. `get_user_model()`
`get_user_model()` returns the currently active user model.


### Do you want to use TokenAuthentication?
To use `TokenAuthentication`, You need to create a token for each user. You don't need to create a Token model manually; it's provided by DRF.
```python
from rest_framework.authtoken.models import Token
```

Then, Where could it be put in?
You can place the token creation in (models.py, serializers.py, views.py). Below are some examples.

In `models.py`:
Automatically create a token when a new user is saved.
```python
@receiver(post_save, sender=settings.AUTH_USER_MODEL)
def create_auth_token(sender, instance=None, created=False, **kwargs):
    if created:
        Token.objects.create(user=instance)
```

In `serializers.py`:
Put it in the class where you'll use.
Create a token when a new user is created.
```python
    def save(self, **kwargs):
        newuser = User.objects.create_user(self.validated_data['username'],
                                        self.validated_data['email'],
                                        self.validated_data['password'])
        newuser.save()
        token = Token.objects.create(user=newuser)
```

And Handling Token Creation in `views.py`
Remember that create function will be only once.
If there are more than one, will return IntegrityError.
And you don't need to create new token after signup.

You can return token value like below.
```python
def create(self, request, *args, **kwargs):
    serializer = self.get_serializer(data=request.data)
    serializer.is_valid(raise_exception=True)

    try:
        user = serializer.save()
    except IntegrityError as e:
        logger.error(f"IntegrityError during user creation: {e}")

    data = {'user_id': user.pk, 'email': user.email}
    token = Token.objects.get(user=user)

    if token:
        data['token'] = token.key

    headers = self.get_success_headers(serializer.data)
    return Response(data, status=..., headers=headers)
```


# Match the user - Login Serializer
You can validate user credentials using Django's authentication system.
```python
from django.contrib.auth import authenticate

user = authenticate(username="test", password="test")
if user:
    return "Successful Login"
else:
    return "Failed Login"
```
`authenticate()` is verify a set of 'user'. If it was validated will return a User object.

If you want to get the data from 'request' you can get through parameter. I naming request to 'data'.
```python
def validate(self, data):
    user = authenticate(username=data['username'],
                        password=data['password'])
```
Then, How can you get the data from client's input?

You can retrieve the user's input from 'request.data' in your views.
```python
class LoginView(generics.ListCreateAPIView):
    serializer_class = LoginSerializer
    queryset = User.objects.all()

    def post(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
```

# permission
You can set permissions for each methods in your views.

Below are examples of how to set permissions
using FBV and CBV.
1. If you just need only one permission you can set like this.

1) Example of an FBV!
```python
from rest_framework.permissions import IsAuthenticated, AllowAny

@api_view(["POST"])
@permission_classes([AllowAny])
def signup(request):
    ...
```

2) Example of an CBV!
```python
class SignupView(generics.CreateAPIView):
    permission_classes = [AllowAny]

class LoginView(APIView):
    permission_classes = [AllowAny]
```

2. You can use 'get_permissions()' method.
(If you need permissions each methods)
```python
class SignupView(APIView):
    def get_permissions(self):
        if self.request.method == 'GET':
            return [AllowAny()]
        return [IsAuthenticated()]
```

3. Finally, If you need custom permission. You can create custom permissions by overriding the 'has_permission' or 'has_object_permission' methods.

You can write in views.py or permissions.py.
Or create a new permissions.py file in the app.
```python
class IsAuthorOrReadOnly(permissions.BasePermission):
    def has_object_permission(self, request, view, obj):
        if request.method in permissions.SAFE_METHODS:
            return True
        return obj.author == request.user
```
obj is from Database. if "database's user information" is "client's user information" then return True. Then we can use API.

# FBV and CBVs
I tried various ways to create API.

## signup API
### FBV
```python
@api_view(["POST"])
@permission_classes([AllowAny])
def signup(request):
    serializer = UserSerializer(data=request.data)
    if serializer.is_valid():
        serializer.save()
        user = User.objects.get(username=request.data['username'])
        token = Token.objects.get(user=user)

        serializer = UserSerializer(user)
        data = {
            'user': serializer.data,
            'token': token.key
        }
        return Response(data, status=status.HTTP_201_CREATED)
    return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

### CBVs
1. APIView
```python
class SignupView(APIView):
    permission_classes = [AllowAny]

    def post(self, request):
        serializer = UserSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            user = User.objects.get(username=request.data['username'])
            token = Token.objects.get(user=user)

            data = {
                "user": serializer.data,
                "token": token.key
                }
            return Response(data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

2. generics
```python
class SignupView(generics.CreateAPIView):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [AllowAny]

    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        serializer.save()
        user = User.objects.get(username=request.data['username'])
        token = Token.objects.get(user=user)

        data = {
            'token': token.key,
            'user_id': user.pk,
            'email': user.email
        }
        headers = self.get_success_headers(serializer.data)
        return Response(data, status=status.HTTP_201_CREATED, headers=headers)
```

3. ViewSet
```python
class UserViewSet(viewsets.ViewSet):
    serializer_class = UserSerializer
    authentication_classes = [TokenAuthentication]

    def get_permissions(self):
        if self.action in ['signup', 'login', '']:
            return [AllowAny()]
        return [IsAuthenticated()]

    def get_queryset(self):
        return User.objects.all()

    def create(self, request, *args, **kwargs):
        serializer = self.serializer_class(data=request.data)
        if serializer.is_valid():
            serializer.save()
            data = {"user": serializer.data}
            token = Token.objects.get(request.data['username'])
            if token:
                data['token'] = token.key
                return Response(data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```



## login API
### FBV
```python
@api_view(["POST"])
def login(request):
    print("FBV Login Method!")
    serializer = LoginSerializer(data=request.data)
    if serializer.is_valid():
        user = User.objects.get(username=request.data['username'])
        token = Token.objects.get(user=user)

        serializer = UserSerializer(user)
        data = {
            'user': serializer.data,
            'token': token.key
        }
        return Response(data, status=status.HTTP_200_OK)
    return Response({"detail": "not found"}, status=status.HTTP_400_BAD_REQUEST)
```

### CBVs
1. APIView
```python
class LoginAPIView(APIView):
    def post(self, request, format=None):
        serializer = LoginSerializer(data=request.data)

        if serializer.is_valid():
            user = serializer.validated_data
            token = Token.objects.get(user=user)
            data = {
                'id': user.id,
                'username': user.username,
                'token': token.key
            }
            return Response(data, status=status.HTTP_200_OK)
        return Response(status=status.HTTP_400_BAD_REQUEST)
```

2. generics
```python
class LoginView(generics.ListCreateAPIView):
    serializer_class = LoginSerializer
    queryset = User.objects.all()
    permission_classes = [AllowAny]

    def post(self, request, *args, **kwargs):
        serializer = self.serializer_class(data=request.data)
        serializer.is_valid(raise_exception=True)
        user = serializer.validated_data

        if user:
            token = Token.objects.get(user=user)
            user_serializer = UserSerializer(user)
            data = {
                'user': user_serializer.data,
            }
        if token:
            data['token'] = token.key
        return Response(data)
```

# generics. ListAPIView VS. RetrieveAPIView

- return many Objects VS. return a specific objects
- using get_queryset() VS. using get_object()
- return objects's list VS. return a object's detail
- serializer(many=True) VS. Default many=False

# Request Examples

```bash
curl -X GET http://localhost:8000/api/accounts/ \
     -H "Authorization: Token c5e1d6e9f5ac3f5477bdb39b812f40d921217096"
```

request.data: {}
request.user: testuser
request.headers: {'Cookie': '', 'Content-Type': 'application/octet-stream', 'Authorization': 'Token 4fa1d50238c3ac20e730d21caa015ae4a7f182db'}
request.auth: 4fa1d50238c3ac20e730d21caa015ae4a7f182db

# How `request.user` works with Token
1. The clident includes the token in the request header
2. DRF uses the 'TokenAuthentication' class to validate this token of the header.
3. If the token is valid, the corresponding user is identified.

4. When a valid token is provided, 'request.user' represents the user instance associated with that token.


