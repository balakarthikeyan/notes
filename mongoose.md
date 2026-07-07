## Mongodb:
    MongoDB is a document database. It's NoSQL.

## CRUD Operations:

- Create:

    ```js
    db.student.insert({
        regNo: "3014",
        name: "Test Student",
        courseName: "MCA",
        duration: "3 Years",
        city: "Bangalore",
        state: "KA",
        country: "India"
    })
    ```

- Read:

    ```js
    db.student.find()
    db.student.find({"regNo": "3014"})
    ```

- Update:

    ```js
    db.student.update(
        {
        "regNo": "3014"	
        },
        $set:
        {
            "name":"Viraj"
        }
    )

- Delete:

    ```js
    db.collection_name.remove({"fieldname":"value"})
    db.student.remove({"regNo":"3014"})
    ```

## MongoDB - Projection:

MongoDB's find() method, accepts second optional parameter that is list of fields that you want to retrieve.

Syntax: `db.COLLECTION_NAME.find({},{KEY:1})`

Please note _id field is always displayed while executing find() method, if you don't want this field, then you need to set it as 0.

## Mongoose:

It is a npm package to connect your express app to mongodb.
Mongoose provides a straight-forward, schema-based solution to model your application data.

```bash
npm install mongoose --save.

const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017-/my_database', 
{
    useNewUrlParser: true,
    useUnifiedTopology: true
});

const Schema = mongoose.Schema;

const BlogPost = new Schema({
    author: String,
    title: String,
    body: String,
    date: { type: Date, default: Date.now }
});
```

## More commands for MongoDB

```js
show dbs;
show collections;
use database_name;
db.table_name.create({"name":"bala"});
db.student.insert({"name":"vivek"});
db.student.find();
db.student.find().pretty();
db.student.find({"name":"bala"});
db.student.update({"name":"vivek"}, {$set : {"name":"satis"}});
db.student.remove({"name":"bala"});
db.student.drop();
git config --global http.proxy http://webproxy.uaeexchange.com:8080 
git config --global http.proxy http://proxyuser:proxypwd@proxy.server.com:8080
```