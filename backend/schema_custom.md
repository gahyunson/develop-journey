# Schema customization
The `@extend_schema_view` decorator doesn't affect the view logic works. It only modifies the API documentation.

`@extend_schema_view`: This decorator allows to Extend the auto generate schema that's created by Django Rest Framework Spectacular.
`list=extend_schema`: Extend the schema for the list endpoint.
`parameters`: It can be pass into the request that made to the list API for this view.
`OpenApiParameter`: Parameter class provided DRF spectacular. It allows us to specify the details of a parameter that can be accepted in the API request.
First parameter 'tags': Specifying the name of the parameter to filter.
`OpenApiTypes.STR`: The type is a String.
`description`: Help to user.

```python
@extend_schema_view(
    list=extend_schema(
        parameters=[
            OpenApiParameter(
                'tags',
                OpenApiTypes.STR,
                description='Comma separated list of IDs to filter',
            ),
            OpenApiParameter(
                'ingredients',
                OpenApiTypes.STR,
                description='Comma separated list of ingredient IDs to filter',
            )
        ]
    )
)
```
