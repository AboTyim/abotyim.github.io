---
title: "Django ORM"

description: "Django ORM — Object Relation Mapper is a powerful and elegant way to interact with the database. The Django ORM is an abstraction layer that allows us to play with the database."

summary: "Django ORM — Object Relation Mapper is a powerful and elegant way to interact with the database."

slug: "orm"
aliases: 
- Django ORM

author: ["AboTyim"]
date: "2021-10-21"
lastmod: "2021-10-21"
tags: ["Django", "Django-ORM", "ORM"]
categories: ["Django"]
series: ["Backend"]

hidemeta: false
hideSummary: false
disableHLJS: false # to disable highlightjs
disableShare: false
#canonicalURL: "https://canonical.url/to/page"

ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
searchHidden: false
ShowToc: true
TocOpen: true
comments: true

cover:
    image: "posts/python/django-orm.jpg"
    alt: "Django Framework ORM"
    caption: "logo from channel very academy"
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page


weight: 20
---



## Methods That Return QuerySets:

| **Method**                | **Description**                                              |
| ------------------------- | ------------------------------------------------------------ |
| **`filter()`**            | Filter by the given lookup parameters. Multiple parameters are joined by SQL `AND` statements |
| **`exclude()`**           | Filter by objects that don’t match the given lookup parameters |
| **`order_by()`**          | Change the default ordering of the QuerySet                  |
| **`reverse()`**           | Reverse the default ordering of the QuerySet                 |
| **`distinct()`**          | Perform an **SQL `SELECT DISTINCT`** query to eliminate duplicate rows |
| **`values()`**            | Returns dictionaries instead of model instances              |
| **`values_list()`**       | Returns tuples instead of model instances                    |
| **`dates()`**             | Returns a QuerySet containing all available dates in the specified date range |
| **`datetimes()`**         | Returns a QuerySet containing all available dates in the specified date and time range |
| **`none()`**              | Create an empty QuerySet                                     |
| **`all()`**               | Return a copy of the current QuerySet                        |
| **`union()`**             | Use the **SQL `UNION`** operator to combine two or more QuerySets |
| **`intersection()`**      | Use the **SQL `INTERSECT`** operator to return the shared elements of two or more QuerySets |
| **`difference()`**        | Use the **SQL `EXCEPT`** operator to return elements in the first QuerySet that are not in the others |
| **`select_related()`**    | Select all related data when executing the query (except many-to-many relationships) |
| **`prefetch_related()`**  | Select all related data when executing the query (including many-to-many relationships) |
| **`defer()`**             | Do not retrieve the names fields from the database. Used to improve query performance on complex datasets |
| **`only()`**              | Opposite of **`defer() : `**return only the name fields      |
| **`using()`**             | Select which database the QuerySet will be evaluated against (when using multiple databases) |
| **`select_for_update()`** | Return a QuerySet that will lock rows until the end of the transaction |
| **`raw()`**               | Execute a raw SQL statement                                  |
| **`AND (&)`**             | Combine two QuerySets with the SQL `AND` operator. Using **`AND (&)`** is functionally equivalent to using **`filter()`** with multiple parameters |
| **`OR` (\|)**             | Combine two QuerySets with the **SQL `OR`** operator         |
| **`annotate()`**          | Annotate each object in the QuerySet. Annotations can be simple values, a field reference or an aggregate expression |

Let’s go ahead and use the Django interactive shell to explore a few examples of the more common QuerySet methods.



### `filter()`

```python
queryset = User.objects.filter(first_name='Ahmed')
queryset
<QuerySet [<User: Ahmed>, <User: Ahmed>, <User: Ahmed>]>
```



### `exclude()`

return a QuerySet of objects that don’t match the given lookup parameters, for example:

```python
>>> from events.models import Venue
>>> Venue.objects.exclude(name="South Stadium")
<QuerySet [<Venue: West Park>, <Venue: North Stadium>, <Venue: East Park>]>
```

Using more than one lookup parameter will use an SQL `AND` operator under the hood:

```python
>>> from events.models import Event
>>> import datetime
>>> venue1 = Venue.objects.get(name="East Park")
>>> Event.objects.exclude(venue=venue1,event_date=datetime.date(2019,10,8))
<QuerySet [<Event: Test Event>, <Event: Club Presentation - Juniors>, <Event: Club Presentation - Seniors>, <Event: Gala Day>]>
```



### `order_by()` and `reverse()`

**`order_by()`** changes the default ordering of the QuerySet. Function parameters are the model fields to use to order the QuerySet. Ordering can be single level:

```python
>>> from events.models import Event
>>> Event.objects.all().order_by('name')
<QuerySet [<Event: Club Presentation - Juniors>, <Event: Club Presentation - Seniors>, <Event: Gala Day>, <Event: Test Event>]>
```

Or ordering can be multi-level. In the following example, the events are first ordered by event date and then by event name:

```python
>>> Event.objects.all().order_by('event_date','name')  
<QuerySet [<Event: Club Presentation - Juniors>, <Event: Club Presentation - Seniors>, <Event: Gala Day>, <Event: Test Event>]>
```

By default, QuerySet fields are ordered in ascending order. To sort in descending order, use the negative sign**(`-`)**:

```python
>>> Event.objects.all().order_by('-name') 
<QuerySet [<Event: Test Event>, <Event: Gala Day>, <Event: Club Presentation - Seniors>, <Event: Club Presentation - Juniors>]>
```

**`reverse()`** reverses the default ordering of the QuerySet:

```python
>>> Event.objects.all().reverse()                     
<QuerySet [<Event: Test Event>, <Event: Club Presentation - Juniors>, <Event: Club Presentation - Seniors>, <Event: Gala Day>]>
```

**Note:**

- A model must have default ordering (by setting the `ordering` option of the models `Meta` class) for `reverse()` to be useful. If the model is unordered, the sort order of the returned QuerySet will be meaningless.
- Both `order_by()` and `reverse()` are not free operations—they come at a time cost to your database and should be used sparingly on large datasets.



### `values()` and `values_list()`

**`values()`** returns Python **dictionaries**, instead of a **QuerySet** object:

```python
>>> Event.objects.values()       
<QuerySet [{'id': 1, 'name': 'Test Event', 'event_date': datetime.datetime(2019, 8, 25, 22, 42, 15, tzinfo=<UTC>), 'venue_id': 1, 'manager_id': 1, 'description': "It's all happening here!"}, {'id': 2, 'name': 'Club Presentation - Juniors', 'event_date': datetime.datetime(2019, 8, 1, 12, 0, tzinfo=<UTC>), 'venue_id': 4, 'manager_id': 2, 'description': ''}]>
```

You can also specify which fields you want returned:

```python
>>> Event.objects.values('name','description') 
<QuerySet [{'name': 'Test Event', 'description': "It's all happening here!"}, {'name': 'Club Presentation - Juniors', 'description': ''}]>
```

**`values_list()`** is the same as **`values()`**, except it returns **tuples**:

```python
>>> Event.objects.values_list() 
<QuerySet [(1, 'Test Event', datetime.datetime(2019, 8, 25, 22, 42, 15, tzinfo=<UTC>), 1, 1, "It's all happening here!"), (2, 'Club Presentation - Juniors', datetime.datetime(2019, 8, 1, 12, 0, tzinfo=<UTC>), 4, 2, '')]>
```

You can also specify which fields to return:

```python
>>> Event.objects.values_list('name')
<QuerySet [('Test Event',), ('Club Presentation - Juniors',)]>
```



### `dates()` and `datetimes()`

You use the **`dates()`** and **`datetimes()`** methods to return time-bounded records from the database (for example, all the events occuring in a particular month). For **`dates()`**, these time bounds are **`year`**, **`month`**, **`week`** and **`day`**. **`datetimes()`** adds **`hour`**, **`minute`** and **`second`** bounds. Some examples:

```python
>>> from events.models import Event
>>> Event.objects.dates('event_date', 'year')
<QuerySet [datetime.date(2019, 1, 1)]>
>>> Event.objects.dates('event_date', 'month') 
<QuerySet [datetime.date(2019, 8, 1)]>
>>> Event.objects.dates('event_date', 'week')  
<QuerySet [datetime.date(2019, 7, 29), datetime.date(2019, 8, 5), datetime.date(2019, 8, 19)]>
>>> Event.objects.dates('event_date', 'day')  
<QuerySet [datetime.date(2019, 8, 1), datetime.date(2019, 8, 10), datetime.date(2019, 8, 11), datetime.date(2019, 8, 25)]>
```



### `union()`

The UNION operator is used to combine the result-set of two or more  querysets. The querysets can be from the same or from different models. When they  querysets are from different models, the fields and their datatypes  should match.

```python
>>> q1 = User.objects.filter(id__gte=5)
>>> q1
<QuerySet [<User: Ritesh>, <User: Billy>, <User: Radha>, <User: sohan>, <User: Raghu>, <User: rishab>]>
>>> q2 = User.objects.filter(id__lte=9)
>>> q2
<QuerySet [<User: yash>, <User: John>, <User: Ricky>, <User: sharukh>, <User: Ritesh>, <User: Billy>, <User: Radha>, <User: sohan>, <User: Raghu>]>
>>> q1.union(q2)
<QuerySet [<User: yash>, <User: John>, <User: Ricky>, <User: sharukh>, <User: Ritesh>, <User: Billy>, <User: Radha>, <User: sohan>, <User: Raghu>, <User: rishab>]>
>>> q2.union(q1)
<QuerySet [<User: yash>, <User: John>, <User: Ricky>, <User: sharukh>, <User: Ritesh>, <User: Billy>, <User: Radha>, <User: sohan>, <User: Raghu>, <User: rishab>]>
```

Since `Hero` and `Villain` both have the `name` and `gender`, we can use `values_list` to limit the selected fields then do a union.

```python
Hero.objects.all().values_list(
    "name", "gender"
).union(
Villain.objects.all().values_list(
    "name", "gender"
))
```

This would give you all `Hero` and `Villain` objects with their name and gender.



### `select_related()` and `prefetch_related()`

Selecting related information can be a database-intensive operation as each  foreign key relationship requires an additional database lookup. For  example, each `Event` object in our database has a foreign key realtionship with the `Venue` table:

```python
>>> event1 = Event.objects.get(id=1)
>>> event1.venue # Foreign key retrieval causes additional database hit
<Venue: South Stadium>
```

For our simple example, this is not a problem, but in large databases with many foreign key relationships, the load on the database can be  prohibitive.

You use **`select_related()`** to improve database performance by retrieving all related data the first time the database is hit:

```python
>>> event2 = Event.objects.select_related('venue').get(id=2) 
>>> event2.venue # venue has already been retrieved. Database is not hit again.
<Venue: East Park>
```

**`prefetch_related()`** works the same way as **`select_related()`**, except it will work across many-to-many relationships.



### `only()`

```python
>>> queryset = User.objects.filter(
    first_name__startswith='R'
).only("first_name", "last_name")
```

The only difference between `only` and `values` is `only` also fetches the `id`.



### `annotate()`

Annotations can be simple values, a field reference or an aggregate expression. For example, let’s use Django’s `Count` aggregate function to annotate our Event model with a total of all users attending each event:

```python
>>> from events.models import Event
>>> from django.db.models import Count
>>> qry = Event.objects.annotate(total_attendees=Count('attendees'))
>>> for event in qry:
...     print(event.name, event.total_attendees) 
... 
Test Event 0
Gala Day 2
Club Presentation - Juniors 5
Club Presentation - Seniors 3
>>>
```



### `raw()`

**Executing Raw SQL**

While Django’s developers provide the **`raw()`** query method for executing raw SQL, you are explicitly discouraged from doing so.

The Django ORM is very powerful and in the vast majority of cases where I  have seen programmers resort to SQL it has been due to an incomplete  knowledge of Django’s ORM on the programmers part, not a deficiency in  the ORM.

If you find yourself in the situation where a query is so complex you can’t find a way of completing the task with Django’s ORM,  it’s likely you need to create a stored procedure or a new view within  the database itself.





## Methods That Don’t Return QuerySets:

| Method                   | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| **`get()`**              | Returns a single object. Throws an error if lookup returns multiple objects |
| **`create()`**           | Shortcut method to create and save an object in one step     |
| **`get_or_create()`**    | Returns a single object. If the object doesn’t exist, it creates one |
| **`update_or_create()`** | Updates a single object. If the object doesn’t exist, it creates one |
| **`bulk_create()`**      | Insert a list of objects in the database                     |
| **`bulk_update()`**      | Update given fields in the listed model instances            |
| **`count()`**            | Count the number of objects in the returned QuerySet. Returns an integer |
| **`in_bulk()`**          | Return a QuerySet containing all objects with the listed IDs |
| **`iterator()`**         | Evaluate a QuerySet and return an iterator over the results. Can improve  performance and memory use for queries that return a large number of  objects |
| **`latest()`**           | Return the latest object in the database table based on the given field(s) |
| **`earliest()`**         | Return the earliest object in the database table based on the given field(s) |
| **`first()`**            | Return the first object matched by the QuerySet              |
| **`last()`**             | Return the last object matched by the QuerySet               |
| **`aggregate()`**        | Return a dictionary of aggregate values calculated over the QuerySet |
| **`exists()`**           | Returns `True` if the QuerySet contains any results          |
| **`update()`**           | Performs an SQL `UPDATE` on the specified field(s)           |
| **`delete()`**           | Performs an SQL `DELETE` that deletes all rows in the QuerySet |
| **`as_manager()`**       | Return a `Manager` class instance containing a copy of the QuerySet’s methods |
| **`explain()`**          | Returns a string of the QuerySet’s execution plan. Used for analysing query performance |

Let’s return to the Django interactive shell to dig deeper into some common examples:



### `get_or_create()`

**`get_or_create()`** will attempt to retrieve a record matching the search fields. If a record  doesn’t exist, it will create one. The return value will be a tuple—the  created or retrieved object and a boolean value that will be `True` if a new record was created:

```python
>>> from events.models import MyclubUser 
>>> usr, boolCreated = MyclubUser.objects.get_or_create(first_name='John', last_name='Jones', email='johnj@example.com')
>>> usr
<MyclubUser: John Jones>
>>> boolCreated
True
```

If we try and create the object a second time, it will retrieve the new record from the database instead.

```python
>>> usr, boolCreated = MyclubUser.objects.get_or_create(first_name='John', last_name='Jones', email='johnj@example.com')
>>> usr
<MyclubUser: John Jones>
>>> boolCreated
False
```



### `update_or_create()`

**`update_or_create()`** works similar to **`get_or_create()`**, except you pass the search fields and a dictionary named `defaults` that contains the fields to update. If the object doesn’t exist, it will create a new record in the database:

```python
>>> usr, boolCreated = MyclubUser.objects.update_or_create(first_name='Mary', last_name='Jones', defaults={'email':'maryj@example.com'}) 
>>> usr
<MyclubUser: Mary Jones>
>>> boolCreated
True
```

If the record does exist, Django will update all fields listed in the `defaults` dictionary:

```python
>>> usr, boolCreated = MyclubUser.objects.update_or_create(first_name='Mary', last_name='Jones', defaults={'email':'mary_j@example.com'})    
>>> usr
<MyclubUser: Mary Jones>
>>> usr.email
'mary_j@example.com'
>>> boolCreated
False
```



### `bulk_create()` and `bulk_update()`

The **`bulk_create()`** saves time by inserting multiple objects into the database at once, most  often in a single query. The function has on required parameter—a list  of objects:

```python
>>> usrs = MyclubUser.objects.bulk_create([
...     MyclubUser(first_name='Jane', last_name='Smith', email='janes@example.com'),
...     MyclubUser(first_name='Steve', last_name='Smith', email='steves@example.com'),
... ])
```

**`bulk_update()`** on the other hand takes a  list of model objects and updates individual fields on selected model  instances. For example, let’s say the first two “Smiths” in the database were entered incorrectly. First, we retrieve all the “Smiths”:

```python
>>> usrs = MyclubUser.objects.filter(last_name='Smith')
>>> usrs
<QuerySet [<MyclubUser: Joe Smith>, <MyclubUser: Jane Smith>, <MyclubUser: Steve Smith>]>
```

We can then modify the **`last_name`** field on the first two instances and use the **`bulk_update`** function to save the changes to the database in a single query:

```python
>>> usrs[0].last_name = 'Smythe' 
>>> usrs[1].last_name = 'Smythe'
>>> MyclubUser.objects.bulk_update(usrs, ['last_name'])
>>> usrs
<QuerySet [<MyclubUser: Joe Smythe>, <MyclubUser: Jane Smythe>, <MyclubUser: Steve Smith>]>
```



### `count()`

Count the number of objects in the QuerySet. Can be used to count all the objects in a database table:

```python
>>> MyclubUser.objects.count()
9
```

Or used to count the number of objects returned by a query:

```python
>>> MyclubUser.objects.filter(last_name='Smythe').count() 
2
```

**`count()`** is functionally equivalent to using the **`aggregate()`** function, for example:

```python
>>> MyclubUser.objects.all().aggregate(Count('id'))
{'id__count': 9}
```

But **`count()`** has a cleaner syntax and is likely to be faster on larger datasets.



### `in_bulk()`

**`in_bulk()`** takes a list of id values and returns a dictionary mapping each id to an  instance of the object with that id. If you don’t pass a list `to **in_bulk()**`, all objects will be returned:

```python
>>> MyclubUser.objects.in_bulk()
{1: <MyclubUser: Joe Smythe>, 2: <MyclubUser: Jane Doe>, 3: <MyclubUser: John Jones>}
```

Once retrieved, you can access each object by their key value:

```python
>>> usrs[3]
<MyclubUser: John Jones>
>>> usrs[3].first_name   
'John'
```

Any non-empty list will retrieve all records with the listed ids:

```python
>>> MyclubUser.objects.in_bulk([1]) 
{1: <MyclubUser: Joe Smythe>}
```

List ids don’t have to be sequential either:

```python
>>> MyclubUser.objects.in_bulk([1, 3, 7]) 
{1: <MyclubUser: Joe Smythe>, 3: <MyclubUser: John Jones>, 7: <MyclubUser: Mary Jones>}
```



### `latest()` and `earliest()`

Return the latest or the earliest date in the database for the provided field(s):

```python
>>> from events.models import Event
>>> Event.objects.latest('event_date')
<Event: Test Event>
>>> Event.objects.earliest('event_date') 
<Event: Club Presentation - Juniors>
```



### `first()` an `last()`

Return the first or last object in the QuerySet:

```python
>>> Event.objects.first()
<Event: Test Event>
>>> Event.objects.last()  
<Event: Gala Day>
```



### `aggregate()`

Returns a dictionary of aggregate values calculated over the QuerySet. For example:

```python
>>> from django.db.models import Count
>>> Event.objects.aggregate(Count('attendees'))
{'attendees__count': 7}
>>>
```

For a list of all aggregate functions available in Django, see Aggregate Functions later in this chapter.



### `exists()`

Returns `True` if the returned QuerySet contains any objects, `False` if the QuerySet is empty. There are two common use cases—to check if an object is contained in another QuerySet:

```python
>>> from events.models import MyclubUser

# Let's retrieve John Jones from the database
>>> usr = MyclubUser.objects.get(first_name='John', last_name='Jones')

# And check to make sure he is one of the Joneses
>>> joneses = MyclubUser.objects.filter(last_name='Jones') 
>>> joneses.filter(pk=usr.pk).exists() 
True
```

And to check if a query returns an object:

```python
>>> joneses.filter(first_name='Mary').exists()
True
>>> joneses.filter(first_name='Peter').exists() 
False
```



## Field Lookups:

Field lookups have a simple double-underscore syntax:

```python
<searchfield>__<lookup>
```

For example:

```python
>>> MyclubUser.objects.filter(first_name__exact="Sally")
<QuerySet [<MyclubUser: Sally Jones>]>
>>> MyclubUser.objects.filter(first_name__contains="Sally") 
<QuerySet [<MyclubUser: Sally Jones>, <MyclubUser: Sally-Anne Jones>]>
```

Under the hood, Django creates SQL `WHERE` clauses to  construct database queries from the applied lookups. Multiple lookups  are allowed and field lookups can also be chained (where logical):

```python
>>> from events.models import Event

# Get all events in 2019 that occur before September
>>> Event.objects.filter(event_date__year=2019, event_date__month__lt=9)
<QuerySet [<Event: Test Event>, <Event: Club Presentation - Juniors>, <Event: Club Presentation - Seniors>, <Event: Gala Day>]>
>>>

# Get all events occuring on or after the 10th of the month
>>> Event.objects.filter(event_date__day__gte=10) 
<QuerySet [<Event: Test Event>, <Event: Club Presentation - Seniors>, <Event: Gala Day>]>
```


| Filter                         | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| **`exact`/`iexact`**           | Exact match. `iexact` is case-insensitive version            |
| **`contains`/`icontains`**     | Field contains search text. `icontains` is case-insensitive version |
| **`in`**                       | In a given iterable (list, tuple or QuerySet)                |
| **`gt`/`gte`**                 | Greater than/greater than or equal                           |
| **`lt`/`lte`**                 | Less than/less than or equal                                 |
| **`startswith`/`istartswith`** | Starts with search string. `istartswith` is case-insensitive version |
| **`endswith`/`iendswith`**     | Ends with search string. `iendswith`is case-insensitive version |
| **`range`**                    | Range test. Range includes start and finish values           |
| **`date`**                     | Casts the value as a date. For datetime field lookups        |
| **`year`**                     | Searches an exact year match                                 |
| **`iso_year`**                 | Searches an exact ISO 8601 year match                        |
| **`month`**                    | Searches an exact month match                                |
| **`day`**                      | Searches an exact day match                                  |
| **`week`**                     | Searches an exact week match                                 |
| **`week_day`**                 | Searches an exact day of the week match                      |
| **`quarter`**                  | Searches an exact quarter of the year match. Valid integer range: 1–4 |
| **`time`**                     | Casts the value as a time. For datetime field lookups        |
| **`hour`**                     | Searches an exact hour match                                 |
| **`minute`**                   | Searches an exact minute match                               |
| **`second`**                   | Searches an exact second match                               |
| **`isnull`**                   | Search if field is null. Takes `True` or `False`             |
| **`regex`/`iregex`**           | Regular expression match. `iregex` is case-insensitive version |





## Aggregate Functions:

Django includes seven aggregate functions:

- **`Avg`**. Returns the mean value of the expression
- **`Count`**. Count the number of returned objects
- **`Max`**. Returns the maximum value of the expression
- **`Min`**. Returns the minimum value of the expression
- **`StdDev`**. Returns the population standard deviation of the data in the expression
- **`Sum`**. Returns the sum of all values in the expression
- **`Variance`**. Returns the population variance of the data in the expression

They are translated to the equivalent SQL by Django’s ORM.

Aggregate functions can either be used directly:

```python
>>> from events.models import Event
>>> Event.objects.count()
4
```

Or with the **`aggregate()`** function:

```python
>>> from django.db.models import Count 
>>> Event.objects.aggregate(Count('id'))
{'id__count': 4}
>>>
```

```python
>>> from django.db.models import Avg, Max, Min, Sum, Count
>>> User.objects.all().aggregate(Avg('id'))
{'id__avg': 7.571428571428571}
>>> User.objects.all().aggregate(Max('id'))
{'id__max': 15}
>>> User.objects.all().aggregate(Min('id'))
{'id__min': 1}
>>> User.objects.all().aggregate(Sum('id'))
{'id__sum': 106}
```



## More Complex Queries

### **Query Expressions**

Query expressions describe a computation or value used as a part of another query. There are six built-in query expressions:

- **`F()`**—Represents the value of a model field or annotated column
- **`Func()`**—Base type for database functions like `LOWER` and `SUM`
- **`Aggregate()`**—All aggregate functions inherit from `Aggregate()`
- **`Value()`**—Expression value. Not used directly
- **`ExpressionWrapper()`**—Used to wrap expressions of different types
- **`SubQuery()`**—Add a subquery to a QuerySet

Django supports multiple arithmetic operators with query expressions, including:

- Addition and subtraction
- Multiplication and division
- Negation
- Modulo arithmetic; and
- The power operator

We have already covered aggregation in this chapter, so let’s have a quick look at the other two commonly used query expressions: **`F()`** and **`Func()`**.

### `F()` Expressions:

The two primary uses for **`F()`** expressions is to move computational arithmetic from Python to the database and to reference other fields in the model.

Let’s start with a simple example, say we want to delay the first event in  the event calendar by two weeks. A conventional approach would look like this:

```python
>>> from events.models import Event
>>> import datetime
>>> e = Event.objects.get(id=1)
>>> e.event_date += datetime.timedelta(days=14)
>>> e.save()
```

In this example, Django retrieves the information from the database into memory, uses Python to perform the  computation—in this case, add 14 days to the event date—and then saves  the record back to the database.

For this example, the overhead  for using Python to perform the date arithmetic is not onerous, however  for more complex queries there is a definite advantage to moving the  computational load to the database.

Now let’s see how we accomplish the same task with an **`F()`** expression:

```python
>>> from django.db.models import F
>>> e = Event.objects.get(id=1)    
>>> e.event_date = F('event_date') + datetime.timedelta(days=14)
>>> e.save()
```

In terms of the amount of code necessary to complete the task, we haven’t saved anything, however, by using the **`F()`** expression, Django will now create an SQL query that performs the computational  logic inside the database, rather than in memory with Python.

While this takes a huge load off the Django application in the case of  executing complex computations, there is one drawback—because the  calculations take place inside the database, Django is now out of sync  with the updated state of the database. We can test this by looking at  the `Event` object instance:

```python
>>> e.event_date 
<CombinedExpression: F(event_date) + DurationValue(14 days, 0:00:00)>
```

To retrieve the updated object from the database, we need to use the **`refresh_from_db()`** function:

```python
>>> e.refresh_from_db()
>>> e.event_date
datetime.datetime(2019, 9, 8, 22, 42, 15, tzinfo=datetime.timezone(datetime.timedelta(0), '+0000'))
```

The second use for **`F()`** expressions—referencing other model fields—is straight forward. For example, you can check for  users with the same first and last name:

```python
>>> MyclubUser.objects.filter(first_name=F('last_name'))
<QuerySet [<MyclubUser: Don Don>]>
```

This simple syntax works with all of Django’s field lookups and aggregate functions.



### `Func()` Expressions

**`Func()`** expressions can be used to represent any function supported by the underlying database (e.g. `LOWER`, `UPPER`, `LEN`, `TRIM`, `CONCAT`, etc.). For example:

```python
>>> from events.models import MyclubUser
>>> from django.db.models import F, Func
>>> qry = usrs.annotate(f_upper=Func(F('last_name'), function='UPPER')) 
>>> for usr in qry:
...     print(usr.first_name, usr.f_upper)
... 
Joe SMYTHE
Jane DOE
John JONES
Sally JONES
Sally-Anne JONES
Sarah JONES
Mary JONES
Jane SMYTHE
Steve SMITH
Don DON
>>>
```

Notice how we are using **`F()`** expressions again to reference another field in the **`MyclubUser`** model.



### `Q()` Objects:

Like **`F()`** expressions, a **`Q()`** object encapsulates an SQL expression inside a Python object. **`Q()`** objects are most often used to construct complex database queries by chaining together multiple expressions using **`AND` (`&`)** and **`OR` (`|`)** operators:

```python
>>> from events.models import MyclubUser 
>>> from django.db.models import Q
>>> Q1 = Q(first_name__startswith='J')
>>> Q2 = Q(first_name__endswith='e') 
>>> MyclubUser.objects.filter(Q1 & Q2)
<QuerySet [<MyclubUser: Joe Smythe>, <MyclubUser: Jane Doe>, <MyclubUser: Jane Smythe>]>
>>> MyclubUser.objects.filter(Q1 | Q2) 
<QuerySet [<MyclubUser: Joe Smythe>, <MyclubUser: Jane Doe>, <MyclubUser: John Jones>, <MyclubUser: Sally-Anne Jones>, <MyclubUser: Jane Smythe>, <MyclubUser: Steve Smith>]>
>>>
```

You can also perform **`NOT`** queries using the negate **(`~`)** character:

```python
>>> MyclubUser.objects.filter(~Q2)      
<QuerySet [<MyclubUser: John Jones>, <MyclubUser: Sally Jones>, <MyclubUser: Sarah Jones>, <MyclubUser: Mary Jones>, <MyclubUser: Don Don>]>
>>> MyclubUser.objects.filter(Q1 & ~Q2) 
<QuerySet [<MyclubUser: John Jones>]>
```





## Model Managers:

A `Manager` is a Django class that provides the interface  between database query operations and a Django model. Each Django model  is provided with a **default `Manager`** named **`objects`**. We have been using the default manager every time we query the database, for example:

```python
>>> newevent = Event.objects.get(name="Xmas Barbeque")
```

```python
>>> joneses = MyclubUser.objects.filter(last_name='Jones')
```

In each example, **`objects`** is the **default `Manager`** for the model instance.

You can customize the default **`Manager`** class, by extending the base `Manager` class for the model. The two most common use-cases for customizing the default manager are:

- Adding extra manager methods; and
- Modifying initial QuerySet results.

### Adding Extra Manager Methods:

Extra manager methods add table-level functionality to models. To add row-level functions, i.e. methods that act on single instances of the  model, you use model methods, which we cover in the next section of the  chapter.

Extra manager methods are created by inheriting the `Manager` base class and adding custom functions to the custom `Manager` class. For example, let’s create an extra manager method for the `Event` model that retrieves the total number of events for a particular type of event (e.g. gala day or presentation):

```python
# myclub_rooteventsmodels.py

from django.db import models
from django.contrib.auth.models import User

# ...

1  class EventManager(models.Manager):
2      def event_type_count(self, event_type):
3          return self.filter(name__icontains=event_type).count()
4  
5  
6  class Event(models.Model):
7      name = models.CharField('Event Name', max_length=120)
8      event_date = models.DateTimeField('Event Date')
9      venue = models.ForeignKey(Venue, on_delete=models.CASCADE)
10     manager = models.ForeignKey(User, blank=True, null=True, on_delete=models.SET_NULL)
11     attendees = models.ManyToManyField(MyclubUser)
12     description = models.TextField(blank=True)
13     objects = EventManager()
14
15     def __str__(self):
16         return self.name
```

- In **Line 1**, we’ve entered a new class called `EventManager` that inherits from Django’s **`models.Manager`** base class.
- **Lines 2 and 3** is the extra `Manager` method we’re adding to the model. This new method returns the total number of the specified event type. Note we’re using the `icontains` field lookup to return all events that have the key phrase in the title.
- In **Line 13** we’re replacing the default manager with our new `EventManager` class. Note that `EventManager` inherits from the `Manager` base class, so all the default manager methods like **`all()`** and **`filter()`** are automatically included in the custom **`EventManager()`** class.

Once it has been created, you can use your new manager method just like any other model menthod:

```python
>>> from events.models import Event
>>> Event.objects.event_type_count('Gala Day')
1
>>> Event.objects.event_type_count('Presentation') 
2
```

### Renaming the Default Model Manager

While the base manager for each model is named **`objects`** by default, you can change the name of the default manager in your class  declaration. For example, to change the default manager name for our  Event class from objects to events, we just need to change line 13 in  the code above from:

```python
13     objects = EventManager()
```

To:

```python
13     events = EventManager()
```

Now you can refer to the default manager like so:

```python
>>> from events.models import Event 
>>> Event.events.all()
<QuerySet [<Event: Test Event>, <Event: Club Presentation - Juniors>, <Event: Club Presentation - Seniors>, <Event: Gala Day>]>
>>> 
```

### Overriding Initial Manager QuerySets

To modify what is returned by the default manager QuerySet, you override the `Manager.get_queryset()` method. This is easiest to understand with an example. Let’s say we commonly  have to check what venues are listed in our local city. To cut down on  the number of queries we have to write, we’re going to create a custom  manager for our `Venue` model (changes in bold):

```python
# myclub_rooteventsmodels.py

1  from django.db import models
2  from django.contrib.auth.models import User
3  
4  
5  class VenueManager(models.Manager):
6      def get_queryset(self):
7          return super(VenueManager, self).get_queryset().filter(zip_code='00000')
8  
9  
10 class Venue(models.Model):
11     name = models.CharField('Venue Name', max_length=120)
12     address = models.CharField(max_length=300)
13     zip_code = models.CharField('Zip/Post Code', max_length=12)
14     phone = models.CharField('Contact Phone', max_length=20, blank=True)
15     web = models.URLField('Web Address', blank=True)
16     email_address = models.EmailField('Email Address',blank=True)
17 
18     venues = models.Manager()
19     local_venues = VenueManager()
20 
21     def __str__(self):
22        return self.name

# ...
```

Let’s take a look at the changes:

- **Lines 5 to 7** is the new `VenueManager` class. The structure is the same as the `EventManager` class, except this time we’re overriding the default `get_queryset()` method and returning a filtered list that only contains local venues.
- In **Line 18** I have renamed the default manager to `venues`
- In **Line 19** we’re adding the custom model manager (`VenueManager`)

Note that there is no limit to how many custom managers you can add to a  Django model instance. This makes creating custom filters for commonly  used queries a breeze. Once you have save the [models.py](http://models.py/) file, you can use the custom methods in your code. For example, the default  manager method has been renamed, so you can use the more intuitive `venues`, instead of `objects`:

```python
>>> Venue.venues.all()  
<QuerySet [<Venue: South Stadium>, <Venue: West Park>, <Venue: North Stadium>, <Venue: East Park>]>
```

And our new custom manager is also easily accessible:

```python
>>> from events.models import Venue
>>> Venue.local_venues.all()
<QuerySet [<Venue: West Park>]>
>>>
```





## Model Methods

Django’s `Model` class comes with many built-in methods. We have already used many of them - **`save()`**, **`delete()`**, **`__str__()`** and others. Where manager methods add table-level functionality to Django’s models, model methods add row-level functions that act on individual  instances of the model.

There are two common cases where you want to play with model methods:

1. When you want to add business logic to the model by adding custom model methods; and
2. When you want to override the default behavior of a built-in model method.

### Custom Model Methods

As always, it’s far easier to understand how custom model methods work by writing a couple, so let’s go ahead and modify our `Event` class (changes in bold):

```python
# myclub_rooteventsmodels.py

# ...

1  class Event(models.Model):
2      name = models.CharField('Event Name', max_length=120)
3      event_date = models.DateTimeField('Event Date')
4      venue = models.ForeignKey(Venue, on_delete=models.CASCADE)
5      manager = models.ForeignKey(User, blank=True, null=True, on_delete=models.SET_NULL)
6      attendees = models.ManyToManyField(MyclubUser)
7      description = models.TextField(blank=True)
8      events = EventManager()
9  
10     def event_timing(self, date):
11         if self.event_date > date:
12             return "Event is after this date"
13         elif self.event_date == date:
14             return "Event is on the same day"
15         else:
16             return "Event is befor this date"
17 
18     @property
19     def name_slug(self):
20         return self.name.lower().replace(' ','-')
21 
22     def __str__(self):
23         return self.name
```

Let’s have a look at what’s happening with this new code:

- In **Line 10** I have added a new method called `event_timing`. This is a straight forward method that compares the event date to the  date passed to the method and returns a message stating whether the  event occurs before, on or after the date.
- In **Line 19** I have added another custom method that returns a slugified event name. The **`@property`** decorator on **Line 18** allows us to access the method directly, like an attribute. Without the **`@property`**, you would have to use a method call (**`name_slug()`**).

Let’s test these new methods out in the Django interactive interpreter. Don’t forget to save the model before you start!

First, the **`name_slug`** method:

```python
>>> from events.models import Event 
>>> events = Event.events.all()
>>> for event in events:
...     print(event.name_slug)
... 
test-event
club-presentation---juniors
club-presentation---seniors
gala-day
```

This should be easy to follow. Notice how the **`@property`** decorator allows us to access the method directly like it was an attribute. I.e. **`event.name_slug`** instead of **`event.name_slug()`**.

Now to test the **`event_timing`** method:

```python
>>> from datetime import datetime, timezone
>>> e = Event.events.get(name="Gala Day")
>>> e.event_timing(datetime.now(timezone.utc))          
'Event is befor this date'
>>>
```

Too easy.

**Date and Time in Django**

Something to note when working with dates in Django. Django uses timezone aware  dates, so if you are doing date comparisons like the in any of your  code, not just in class methods, you can’t use `datetime.now()` as Django will throw a `TypeError: can't compare offset-naive and offset-aware datetimes`. To avoid this error, you must provide timezone information with your dates.





### Overriding Default Model Methods

It’s common to want to override built-in model methods like **`save()`** and **`delete()`** to add business logic to default database behavior.

To override a built-in model method you define a new method with the same name. For, example, let’s override the `Event` model’s default **`save()`** method to automatically assign management of the event to a staff member (changes in bold):

```python
# myclub_rooteventsmodels.py

# ...

1  class Event(models.Model):
2      name = models.CharField('Event Name', max_length=120)
3      event_date = models.DateTimeField('Event Date')
4      venue = models.ForeignKey(Venue, on_delete=models.CASCADE)
5      manager = models.ForeignKey(User, blank=True, null=True, on_delete=models.SET_NULL)
6      attendees = models.ManyToManyField(MyclubUser)
7      description = models.TextField(blank=True)
8      events = EventManager()
9  
10     def save(self, *args, **kwargs):
11         self.manager = User.objects.get(username='abbyb')
12         super(Event, self).save(*args, **kwargs)
# ...
```

The new **`save()`** method starts on **Line 10**. In the overridden `save()` method, we’re first assigning the staff member with the username “abbyb” to the `manager` field of the model instance (**Line 11**), and then we call the default `save()` method with the **`super()`** function to save the model instance to the database (**Line 12**).

Once you save your **`models.py`** file, you can test out the overridden model method in the Django interactive shell:

```python
>>> from events.models import Event
>>> from events.models import Venue
>>> from datetime import datetime, timezone
>>> v = Venue.venues.get(id=1)
>>> e = Event.events.create(name='New Event', event_date=datetime.now(timezone.utc), venue=v)
```

Once the new record has been created, you can test to see if your override worked by checking the `manager` field of the `Event` object:

```python
>>> e.manager
<User: abbyb>
>>>
```





## Model Inheritance

Models are simply Python classes, so  inheritance works the same way as normal Python class inheritance. The  two most common forms of model inheritance in Django are:

1. **Multi-table inheritance**, where each model has its own database table; and
2. **Abstract base classes**, where the parent model holds information common to all its child classes, but doesn’t have a database table.

You can also create proxy models that modify the Python-level behavior of a model without modifying the underlying model fields, however, we won’t  be covering them here. See the [Django documentation](https://docs.djangoproject.com/en/2.2/topics/db/models/#proxy-models) for more information on proxy models.



### Multi-table Inheritance

Multi-table inheritance is straight  forward—the parent class is a normal model and the child inherits the  parent by declaring the parent class in the child class declaration. For example:

```python
class MyclubUser(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    email = models.EmailField('User Email')

    def __str__(self):
        return self.first_name + " " + self.last_name


class Subscriber(MyclubUser):
    date_joined = models.DateTimeField()
```

The parent model in the example is the `MyclubUser` model from our `events` app. The `Subscriber` model inherits from `MyclubUser` and adds an additional field (`date_joined`). As they are both standard Django model classes, a database table will be created for each model.



### Abstract Base Classes

Abstract base classes are handy when  you want to put common information into other models without having to  create a database table for the base class.

You create an abstract base class by adding the **`abstract = True`** class `Meta` option (**Line 7** in this example):

```python
1  class UserBase(models.Model):
2      first_name = models.CharField(max_length=30)
3      last_name = models.CharField(max_length=30)
4      email = models.EmailField('User Email')
5  
6      class Meta:
7          abstract = True
8          ordering = ['last_name']
9  
10 
11 class MyclubUser(UserBase):
12     def __str__(self):
13         return self.first_name + " " + self.last_name
14 
15 
16 class Subscriber(UserBase):
17     date_joined = models.DateTimeField()
```

Abstract base classes are also useful for declaring class **`Meta`** options that are inherited by all child models (**Line 8**).

As the `MyclubUser` model from our `events` app now inherits the first name, last name and email fields from `UserBase`, it only needs to declare the **`__str__()`** function to behave the same way as the original **`MyclubUser`** model we created earlier.

This example is very similar to the example for multi-table inheritance in  the previous section, and if you saved and migrated these models, you  would get exactly the same result as the above code —Django would create the **`events_myclubuser`** and **`events_subscriber`** tables in your database, however, because **`UserBase`** is and abstract model, it won’t be added to the database as a table.





## References:

- https://raturi.in/blog/everything-about-django-orm-querysets-field-lookups-model-managers-aggregate-functions-tutorial/
- https://www.youtube.com/playlist?list=PLOLrQ9Pn6cazjoDEnwzcdWWf4SNS0QZml
