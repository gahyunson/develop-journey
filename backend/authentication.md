# Type of Authentication
1. Basic
Basic HTTP authentication
Send username and password with every each request.
So Any request API will include user's username and password with that request.
It means client need to store the user's username and password in clear text.
The worst happen is a attacker can see the actual clear text password.
2. Token
Generate a token from the user's username and password.
Every request that's made to the API include token needs to be authenticated.
- Right balance of simplicity and security
- Simple - Supported out of the box by DRF
- support by most clients.
- The worst happen is a attacker can see only the token. (it's not bad as seeing the clear.)
3. JWT
It works by using a refresh and access token.
Refresh token == the only token that requires verification of the user's credential.
So basically people use this type of authentication when you need something super scalable.
4. Session
Store the authentication using cookies.

# Token
1. Post username/password
2. Create token
3. Store token on client (Session, Local, Cookie, DB)
4. Include token in the HTTP headers (Every request that the client makes to the APIs that have to be authenticated is includes the token)

### Pros and Cons
Pros
- Supported out of the box
- Simple to use
- Supported by all clients
- Stay away from sending username/password each time
Cons
- Token needs to be secured on the client side.
- If the token is compromised or somebody managed to get hold of the token, extract it from the client (the session storage) then they can use the token.
(warning a shared computer)

### Logging out
- Happens on the client side
- By deleting the token

Why no logout API?
- Because it's very unreliable
- e.g.
    - Delete a app while they're logged in, then the app isn't going to able to cool the log out API.
    - Disconnect off the internet.
- Not usefule on API

```python
from django.contrib.auth import authenticate
```
It allows to authenticate with the authentication system.
It can be used like below.
```python
email = attrs.get('email')
password = attrs.get('password')
user = authenticate(
    request=self.context.get('request'),
    username=email,
    password=password,
)
```

