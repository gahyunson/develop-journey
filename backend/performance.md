# Python and Django Performance Oprimization 

## Model Fields indexing

### Indexing Basics
- O(n): If a field isn't indexed, the DB has to scan every row to find that you're looking for. This is called a full table scan, which is costly, especially for large datasets.
- O(1): With indexing, the database can quickly locate data, similar to retrieving from a hash table. It reduces the search time, allowing the databse to directly access the desired rows.
- O(n*log(n)): In some cases, the index behaves like a B-tree.

### Ex. Applying Indexes in Django Models
Declare indexes using the `db_index=True` argument on fields,
or throught the `Meta` class with the `indexes` atrribute. 


```python
class Photo(models.Model):
    photo_path = models.CharField(max_length=200, null=False, blank=False, db_index=True)

    class Meta:
        indexes = [
            models.Index(fields=['photo_path'], name='photo_path_idx'),
        ]

class Price(models.Model):
        indexes = [
            models.Index(fields=['size', 'price']),
            models.Index(fields=['size'], name='size_idx')
        ]
```

The time was '0.6x' and now 0.25. But I retried measure time then I also saw time 0.6x still. 
Of course, the time was 0 when I set cache on this query.