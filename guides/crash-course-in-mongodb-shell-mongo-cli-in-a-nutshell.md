# Crash Course in MongoDB Shell (Mongo CLI)

> This crash course in MongoDB shell is quick summary of official MongoDB
> documentation on using shell.


Links to MongoDB installation guide
-----------------------------------

- [Installing MongoDB with macports on osx](../environment-setup/installing-mongodb-with-macports-on-osx.md)


Database level operations
-------------------------

### Display databse in use

```
db
```

### List of all available databases 

```
show dbs
```

### List of all accessible databases

```
show databases
```

### Switch database

```
use my_test_db
```

### Remove database

```
db.dropDatabase()
```

### Returns reference to another databse

```
db.getSiblingDB()
```


Collection level operations
---------------------------

### List of all collections

```
show collections
```

### Executing queries on `my_collection`

```
db.my_collection.find()
```

### Alternative way to access `my-collection`

**Notice:** a hyphen present in the collection name
```
db["my-collection"].find()
db.getCollection("my-collection").find()
```

### Pretty print

```
db.my_collection.find().pretty()
```

### Main operations available on `db.<collection>`

- `find()` - lists all documents in the collection and **returns cursor** 
- `insert()` - inserts a new document into the collection
- `update()` - updates existing document
- `save()` - inserts either a new or updates an existing document
- `remove()` - deletes documents
- `drop()` - removes entire collection
- `ensureIndex()` - creates new index if one doesn't exist


Write Operations
----------------

### Insert

#### Insert Pattern

> `db.<collection>.insert( { *field* : *value* } )`

Resources: [insert docs](http://docs.mongodb.org/v2.6/reference/method/db.collection.insert/#db.collection.insert)

#### Insert single record

```
db.users.insert({ 
    name: "Tom Smith", 
    email: "tom.smith@gmail.com",
    scores: [22, 31, 47, 48, 50] 
})
```

#### Insert multiple records

```
db.users.insert([
    { name: "Tom Brown", email: "tom.brown@gmail.com", scores: [33, 40, 47] },
    { name: "John Black", email: "j.black@yahoo.com", scores: [45, 32, 55] },
    { name: "Eliot Thomson", email: "e.thomson@gmail.com", scores: [15] }
])
```

### Delete

#### Delete all records from collection

```
db.users.remove({})
```

### Update

#### Update Pattern

> `db.<collection>.update( *criteria*, *action*, *options* )`  
> Resources: [update docs](http://docs.mongodb.org/v2.6/reference/method/db.collection.update/#db.collection.update)  

#### Update operators 

- `$currentDate` - Sets a field value to current date (Date or Timestamp)
- `$inc` - Increments field value by passed value
- `$mul` - Multiplies field value by the specified amount
- `$rename` - Renames a field
- `$set` -  Sets the value of a field
- `$unset` - Removes the specified field

> Resources: [Full list of field update operators](http://docs.mongodb.org/manual/reference/operator/update-field/)

#### Update options

- `upsert` - if set to true, it will create new document if there are none find matching the query criteria. The default value is false
- `multi` - if set to true, updates multiple documents that meet the query criteria

#### Update one document record

Example 1

> **Warning:** operator `$set` has been used, **field value will be replaced**

```
db.users.update({ name: "Tom Smith" }, { $set : {name: "Tommy Smith" }})
```

Example 2

>**Warning:** operator `$set` not used, **document will be replaced**

```
db.users.update({ name: "Tom Smith" }, { name: "Tommy Smith" })
```

#### Update one record with multiple update operations

Field `name` will be remaned to `full_name` and new field `nick` name will be added.
```
db.users.update(
    { name: "Eliot Thomson" }, 
    {         
        $rename : { name: "full_name" },
        $set : { nick: "Ely" }
    }
)
```

#### Update multiple document records

> Update option: `multi = true`  
> **Note:** Field `updated = true` will be added to *multiple* documents containing field: `name`

```
db.users.update(
    { name : { $exists: true }}, 
    { $set: { updated: true }}, 
    { multi: true }
)
```
#### Creates new document if criteria match not found

> Update option: `upsert = true`  
> **Note:** New document will be added if there is no documents matching specified query criteria  

```
db.users.update(
    { email: "simon.bone@gmail.com" },
    { $set: { 
        name: "Simon Bone", 
        pseudo: "Bone", 
        email: "simon.bone@gmail.com" 
    }},
    { upsert: true }
)
```

Read Operations
---------------

### Introduction to query interface to selecting documents

> `db.<collection>.find( *criteria*, *projection* ).*modifier()*`

**Resources:** official docs to [read operations](http://docs.mongodb.org/v2.6/core/read-operations-introduction)

### Selecting documents based on criteria

#### Query criteria (selectors)

**Resources:** Official docs to [query selectors](http://docs.mongodb.org/v2.6/reference/operator/query/#query-selectors)

##### Comparison criteria

- `$gt` - match greater than query value
- `$gte` - match greater than or eaqual to query value
- `$in` - match if value exist in query array 
- `$lt` - match lower than query value
- `$lte` - match lower than or euqal to query value
- `$ne` - match all not equal to query value
- `$nin` - match all not in query array

##### Logical criteria

- `$and` - joins queries with logical AND
- `$nor` - joins queries with logical XOR
- `$not` - negate query with logical ! 
- `$or` - joins queries with logical OR

##### Element criteria

- `$exists` - match documents with existing field  
- `$type` - match documents with field of numeric [BSON type](http://docs.mongodb.org/v2.6/reference/operator/query/type/#available-types)

##### Evaluation criteria

- `$mod` - matches result of modulo operation on the field value
- `$regex` - matches regular expression
- `$text` - matches finding string in fields with *text index*
- `$where` - matches reults of evaluating JavaScript expression

##### Geospatial criteria

- `$geoIntersects` - matches geometries that intersect with a GeoJSON geometry
- `$geoWithin` - matches geometries within a bounding GeoJSON geometry. 
- `$nearSphere` - matches geospatial objects in proximity to a point on a sphere, requires a geospatial index
- `$near` - matches geospatial objects in proximity to a point, requires a geospatial index

##### Array criteria

- `$all` - matches field values containing all elements in the array
- `$elemMatch` -  matches field values specified by array of conditions
- `$size` - matches documents with array field of specified size

##### Comments

- `$comment` - appends a comment to a query predicate. Useful for log entries.


#### Document selecting examples

##### Find documents with existing field name
```
db.users.find({"name" : {$exists: true}})
```

##### Find documents containging text key word

> **warning:** performing $text search requires text index on searched field  
> `db.users.ensureIndex( { name: "text" } )`

```
db.users.find({ $text: { $search: "tom" }})
```

##### Find documents conforming to regular expression

> **note:** below query returns same result as above *text search*  
> **warning:** executing regular expression is always slower than text search

```
db.users.find({"name" : {$regex : /^Tom.*/i}})
```

##### Find documents with specified length of array field
```
db.users.find({"scores" : {$size : 3} })
```

##### Find documents conforming to evaluated JavaScript expression

> **Note:** query returns results same as above *matching array length*  
> **Warning:** evaluating js expression is always slower than native queries

```
db.users.find( { $where: "this.scores.length === 3" } );
```

##### Find documents matching queries joined with logical `$and`

```
db.users.find({ $and: [
    { scores: { $size: 3 } }, 
    {name: { $exists: true }}
]})
```

##### Find documents matching query negation with logical `$not`
```
db.users.find({ scores : { $not: {$size : 3 } } })
```

##### Find documents matching array of conditions with `$elemMatch`
```
db.users.find({ scores: { $elemMatch: { $gt:40, $lt: 50 } } })
```

##### Find combine conditions array matches with `$elemMatch`

> **note:** `$elemMatch` can be used both as query operator and projection operator  
> Resource: interesting [`$elemMatch`](comparison (http://stackoverflow.com/a/21519732)

```
db.users.find({ $and : [
    { scores: { $elemMatch: { $lt: 50 } } },
    { scores: { $not : { $elemMatch: { $lt: 30 } } } },
]})
```
**note:** `$and` queries are evaluate in order. Following query is evaluated only previous query returns results

### Projecting selected document

#### Projection criteria

- `$` - projects only the first element in an array that matches the query condition, use with **array logical criteria**
- `$elemMatch` - projects the first element in an array that matches the specified $elemMatch condition
- `$meta` -  projects the document’s score assigned during $text operation,
- `$slice` - limits the number of elements projected from an array, it supports skip and limit slices.

#### Projection examples

##### Project documents with only one specified field
```
db.users.find({}, {scores: 1})
```

##### Exclude specified field from results
```
db.users.find({}, {scores: 0})
```

##### Return limited number of array elements

> **note:** positive index returns first n number of elements

```
db.users.find({}, {scores: { $slice: 2}})
```

> **note:** negative index returns last n number of elements

```
db.users.find({}, {scores: { $slice: -3}})
```

### Modifying selected and/or projected documents

#### Example cursor modifier methods

> This document lists most often methods, check 
> [official docs](http://docs.mongodb.org/manual/reference/method/js-cursor/)
> for full list of cursor methods

#### Examples of modified document selection

#### Limits document passed to the pipeline
```
db.users.find({}).limit(2)
```

#### Orders document passed to the pipeline

Example 1

> **note:** `{name: 1}` requests ascending sorting

```
db.users.find({}).sort({name: 1})
```

Example 2

> **note:** `{name: 1}` requests descending sorting

```
db.users.find({}).sort({name: -1})
```

#### Combining modifiers

```
db.users.find({}).limit(2).sort({name: -1})
```

#### Order, limit and skip documents passed to the pipeline

Example 1

```
db.users.find().sort({name: 1}).limit(2).skip(2)
```

Example 2

> **note**: changing order of `limit()` and `sort()` doesn't change execution order 

```
db.users.find({}).sort({name: -1}).limit(2)
```

### Agregation

#### Examples of Aggregation Pipeline Operators

> **Resources:** Full list of [Aggregation Pipeline Operators](http://docs.mongodb.org/v2.6/reference/operator/aggregation/)

##### Stage Operators

- `$group` - groups input documents by a specified identifier expression and applies the accumulator expression(s), if specified, to each group
- `$limit` - passes only the first `n` documents to the pipeline
- `$match` - applies filter to documents passed to a pipeline
- `$skip` - skips the first `n` documents
- `$sort` - reorders the document stream by a specified sort key

### Examples of agregated output

~~~ TODO ~~~

Official guides
---------------

- [Tutorial: Getting Started with MongoDB Shell](http://docs.mongodb.org/v2.6/tutorial/getting-started-with-the-mongo-shell/) - v2.6
- [Reference: Mongo Shell](http://docs.mongodb.org/v2.6/reference/mongo-shell/) - v2.6