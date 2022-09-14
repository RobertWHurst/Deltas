# 💎 Deltas

A library for applying and manipulating MongoDB style queries and deltas purely
in JavaScript.

# Introduction

The idea behind this library is to allow the use of MongoDB style queries and
deltas in your JavaScript code, working directly with arrays and objects in
your program -- no MongoDB required.

# Methods

## Apply Delta

Applies a delta to a document (object). Please see
https://docs.mongodb.com/manual/reference/operator/update/ for documentation on
operators you can use. Note that not all operators are implemented, but most
are. Please note that if you are using typescript, operators will properly
effect to shape of the resulting document.

### Supported Operators

- $currentDate
- $inc
- $min
- $max
- $mul
- $rename
- $set
- $setOnInsert (with asInsert set to true)
- $unset
- $addToSet
- $pop
- $pull
- $push
- $pullAll

### Example

```typescript
applyDelta({ number: 10 }, { $inc: 2 }) // returns { number: 12 }
```

### Signature

```
applyDelta(document, delta, options?) -> document
```

### Arguments

**document**
: the document you wish the apply the delta to. Note that this document can be any object.

**delta**
: the delta you wish to the document. The delta must be an object containing mongoDB delta operators.

**options**
: an options object. Can contain the property asInsert to enable $setOnInsert.

## Apply Query

Applies a query to a collection of documents, filtering out ones that do not
meet the query. The matching documents will be returned by the method in a
results array. Please see
https://docs.mongodb.com/manual/reference/operator/query/ for query operators
you can use. Note that not all operators are implemented, but most are.

### Supported Operators

- direct matches
- $eq
- $gt
- $gte
- $in
- $lt
- $lte
- $ne
- $nin
- $exists
- $type
- $mod
- $regex

### Example

```typescript
applyQuery([{ number: 10 }, { text: 'hello world' }], { number: { $gt: 5 } }) // returns [{ number: 10 }]
```

### Signature

```
applyQuery(collection, query) -> filteredCollection
```

### Arguments

**collection**
: the array of documents you wish to filter using the query.

**query**
: the query you wish to use to filter the collection. The query must be an object containing mongoDB query operators.

## Project Delta

Takes a delta and renames the fields effected using a projection, and returns
a new delta with the updated field names.

### Example

```typescript
projectDelta({ $set: { number: 5 } }, { number: 'int' } as const) // returns { $set: { int: 5 } }
```

### Signature

```
projectDelta(delta, projection) -> projectedQuery
```

### Arguments

**delta**
: the delta to modify using the projection.

**projection**
: the projection used to modify the delta.

## Project Query

Takes a query and renames the fields effected using a projection, and returns
a new query with the updated field names.

### Example

```typescript
projectQuery({ number: { $gt: 5 } }, { number: 'int' } as const) // returns { int: { $gt: 5 } }
```

### Signature

```
projectQuery(query, projection) -> projectedQuery
```

### Arguments

**query**
: the query to modify using the projection.

**projection**
: the projection used to modify the query.
