# Welcome to Mongo

[cheatsheet](https://www.mongodb.com/developer/quickstart/cheat-sheet/)

```
in DB:
database
  collections
    documents

In Node:
Mongoose
  schema
    model
      documents
```

It is recommended to always use schemas with mongodb.
How do we connect to a mongodb from node. There are two ways
1. Native node mongodb driver
2. A npm package called mongoose

## Mongoose
1. Require mongoose
```js
const mongoose = require("mongoose")
```
2. Connect to a mongoose database
```js
const connect  =  ()=>{
  // returns a promise = > connect takes in a url
  // url expects the protocal which is mongodb://the host (localhost for machine)/databaseName
  return mongoose.connect("mongodb://localhost:27017/collectionName") 
}
```

## Collections,Document and Models

### Collections

A collection is a grouping of MongoDB documents. Documents within a collection can have different fields. A collection is the equivalent of a table in a relational database system.

Document and Model are distinct classes in Mongoose. The Model class is a subclass of the Document class. When you use the Model constructor, you create a new document. Model helps you create a collection of documents.

### Document

Documents are individual records in a MongoDB collection and are the basic unit of data in MongoDB.

#### Mongoose document

Mongoose documents represent a one-to-one mapping to documents as stored in MongoDB. Each document is an instance of its Model.

```js
{_id:5bb4ad2r3r23r23ree:firstName:"Tim",__v:0}
id => object.id
v => version of the schema
firstName  => field
```

#### Mongoose Models
Models are fancy constructors compiled from Schema definitions. An instance of a model is called a document. Models are responsible for creating and reading documents from the underlying MongoDB database.


#### Mongoose Schema
The shape of the document/data

1. Define shape/schema
```js
const collection = new mongoose.Schema({
  field:dataType
})

// example
const student =  new mongoose.Schema({
  firstName:String,
  SecondName:String,
  age:Number
})
```

4. Convert the schema to a mongo model or mongoose model, which will create a collection for us
```js
const Model = mongoose.model('modelNameInSingular',schema)
//example
const Student = mongoose.model('student',student)
```

Student here is actually a mongoose document (not a mongo document) and not a javascript object. It has a lot of APIs. It cannot be logged out (as these Object is not inumerable.)
Sometimes we might need to convert these documents to JSON as these documents are slow.

These are mongoose documents, not mongo documents. Mongoose wraps around mongo. Mongo just takes and spits JSON. Mongoose wraps around this JSON, virtualises it and provides many helpes/APIs or methods that lets us operate on these documents within the confines of mongoose.

#### Nested data as document

```js
const student = new mongoose.Schema({
  firstName: {
    type: String,
    required:true
  },
  info: {
    school: { type: String ,required: true}
  },
  age: {
    type:Number
  },
  class:[{type:String}]
},{timestamps:true})

```
required:true is a validation whereas unique is an index

#### Document API

find and update

```js
  return User.findIdAndUpdate(id,update,{new:true}) , if you dont pass in the option new:true, it will update and still give you the old object before update.
```

### Query object
Pretty much every query we do on a model returns a query object (looks like promises). They have a `then` method on it. Anything consuming the query thinks it a promise, but it is not a real promise. So  to avoid that, we use .exec() after query to tell that you are done and there is no more further query which returns a real promise.

```js
User.findById(id).exec()
```

This is only for queries, not for create,delete ,update etc but needed for findByIdAndRemove etc

# CRUD OPERATIONS IN MONGOOOSE