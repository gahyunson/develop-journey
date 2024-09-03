### [27/Aug/2024 06:16:36] "PUT /api/cart/ HTTP/1.1" 302 0
A Django redirect results generally in a 302 being returned.
It's up to the Javascript to recognize the 302 response and request.
Especiaaly If you use 'put' method, could happen infinite redirection.

Therefore, change it to JsonResponse if you want to receive 200.

redirect('url') -> JsonResponse(status=status.HTTP_200_OK)

# Redirect to 'main' page when user isn't authenticated
I created a get method in the Cart app that cart list if the user is authenticated.
If the user is not authenticated, they should be redirected to the 'main' page.

Initially, I wrote the code like this.
```python
class CartView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request, *args, **kwargs):
        if request.user.is_authenticated:
            cart = Cart.objects.filter(user=request.user)
            total = self.get_total(cart)
            context = {
                'user': request.user,
                'cart': cart,
                'total': total,
            }
            return render(request, 'cart.html', context)
        else:
            return render(request, 'user')
```
Note that `permission_classes`. I set it to IsAuthenticated.
It checks if the user is authenticated before the get method runs.
If the user is not authenticated, they won't be able to access to get method.

Change it to AllowAny.

```python
class CartView(APIView):
    permission_classes = [AllowAny]
```

