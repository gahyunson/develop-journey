# Delete method and Response Body
You can handle the status code for the method you implemented.
Usually DELETE method returns a 204 status code.
A 204 status code don't have data to send in the response body.

If you need to include information in the response body, use a '200' status code.

# Response Status Code
When returning a response with data, you can omit the status parameter name if you're using HTTP_200_OK.
```python
return Response(serializer.data, status.HTTP_200_OK)
```

However, when returning a response without data, simply passing the status code without the parameter name won't work as expected.
```python
return Response(status.HTTP_204_NO_CONTENT)
```

You must explicitly specify the status parameter name.
```python
return Response(status=status.HTTP_204_NO_CONTENT)
```