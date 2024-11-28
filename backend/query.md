# Query
### create() & save()
create() is a convenience method.
creating + saving all in one step.

```python
cup = Cup.objects.create(name="Blur", nation="China")
```

```python
cup = Cup(name="Blur", nation="China")
cup.save(force_insert=True) # Hit DB
```

### ForeignKey
To add the method on the field when the models are already saved to the database.

```python
>>> from blog.models import Blog, Entry
>>> entry = Entry.objects.get(pk=1)
>>> cheese_blog = Blog.objects.get(name="Cheddar Talk")
>>> entry.blog = cheese_blog
>>> entry.save()
```

### ManyToManyField
```python
>>> from blog.models import Author
>>> joe = Author.objects.create(name="Joe")
>>> entry.authors.add(joe)

>>> john = Author.objects.create(name="John")
>>> paul = Author.objects.create(name="Paul")
>>> george = Author.objects.create(name="George")
>>> ringo = Author.objects.create(name="Ringo")
>>> entry.authors.add(john, paul, george, ringo)
```

Retrieving objects - Managers
A Manager is the interface through which database query operations are provied to Django models.
Managers are accessible only via model classes.

Retrieving all objects
```python
entry = Entry.objects.all()
```

Retrieving specific objects with filters
`get()` Returns only one object not a queryset. It would raise an exception if no object is found.
`filter()` Returns a queryset that match the given lookup parameters. It returns an empty set if no object is found.
`exclude()` Returns objects that do not match the given lookup parameters.

The lookup parameters should be in the format described in Field lookups.
Field lookups? field__lookuptype=value

The field specified in a lookup has to be the name of a model field. There's one exception though, in case of a ForeignKey you can specify the field name suffixed with _id.
```python
Entry.objects.filter(blog_id=4)
```

An invalid keyword argument -> TypeError

exact, iexact, contains, incontains ...

**exact**
```python
Entry.objects.get(headline__exact="Cat bites dog")
```
is equal with
```SQL
SELECT ... WHERE headline = 'Cat bites dog';
```

**iexact**
```python
Blog.objects.get(name__iexact="beatles blog")
```
is equal with "Beatles Blog", "beatles blog", or even "BeAtlES blOG".

**contains**
```python
Entry.objects.get(headline__contains='Lennon')
```
is equal with
```SQL
SELECT ... WHERE headline LIKE '%Lennon%';
```

