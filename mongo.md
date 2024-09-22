## Setup
> C:\Program Files\MongoDB

1. Create two folders here, name them `data` and `log`.
2. Create another folder inside data and name it as `db`

> C:\Program Files\MongoDB\Bin
```
mongod --directoryperdb --dbpath "C:\Program Files\MongoDB\data\db" --logpath "C:\Program Files\MongoDB\log\mongo.log" --logappend --rest --install
```

> Start
```
net start MongoDB
```
> Check to Run
```
mongo
```
> Stop 
```
net stop MongoDB
```
## Commands
```
use database_name;
db;
show dbs;
show collections;
//drop db
db.dropDatabase();
drop database_name;
//create collection
db.createCollection(collection_name, {capped : true, size : 9232768, max: 10, autoIndexId:false});
db.collection_name.insert({
  name: "balakarthikeyan",
  age: 30,
  website: "balakarthikeyan.com"
});
//delete
db.collection_name.remove({'title':'test'});
//find method
db.collection_name.find();
//drop collection
db.collection_name.drop();
db.collection_name.find().forEach(printjson) / db.collection_name.find().pretty();
db.collection_name.find({"field_name":{$gt:criteria_value}}).pretty();
db.collection_name.find({"field_name":{$lt:criteria_value}}).pretty();
db.collection_name.find({"field_name":{$ne:criteria_value}}).pretty();
db.collection_name.update({"name":"Jon Snow"},{$set:{"name":"Kit Harington"}});
db.collection_name.save({_id:ObjectId(), new_document});
db.collection_name.remove(delete_criteria, noOfRecord);
db.collection_name.remove({}); -- For all records
db.collection_name.find({}, {"_id": 0, "student_id": 1}); -- Projection it show only student_id
db.collection_name.find({student_id : {$gt:2002}}).limit(1).pretty();
db.collection_name.find({student_id : {$gt:2002}}).limit(1).skip(1).pretty(); --Skip first record and show remaining
db.collecttion_name.find().sort({field_key:1 or -1});
db.collection_name.createIndex({field_name: 1 or -1});
db.collection_name.getIndexes();
db.collection_name.dropIndex({index_name: 1});
db.collection_name.dropIndexes();
```
> Insert one
```
db.posts.insert({
	title: 'Post One',
	body: 'Body of Post',
	category: 'News',
	likes: 4,
	tags: [ 'news', 'events' ],
	user: {
		name: 'Tom',
		status: 'author'
	},
	date: Date()
});

db.users.insert({
    username: "ramesh",
    password: "1234",
    firstName: "Ramesh",
    lastName: "Ravi",
    age: "31",
    salary: "100000"
});
```
> Insert many
```
db.posts.insertMany([
	{
		title: 'Post Two',
		body: 'Body of Post 2',
		category: 'Technology',
		date: Date()
	},
	{
		title: 'Post Three',
		body: 'Body of Post 3',
		category: 'News',
		date: Date()
	},
	{
		title: 'Post Four',
		body: 'Body of Post 4',
		category: 'Entertainment',
		date: Date()
	}
]);
```
> Find
```
db.posts.find({ category: 'News' });
```
> Sort
```
db.posts.find().sort({ title: 1 });
```
> Count
```
db.posts.find({ category: 'News' }).count();
```
> Limit
```
db.posts.find().limit(2);
```
> Update
```
db.posts.update(
	{ title: 'Post Two' },
	{
		title: 'Post Two',
		body: 'New Body of Post 2',
		category: 'Technology',
		name: 'test update'
	},
	{ upsert: true }
);

db.posts.update(
	{ title: 'Post Two' },
	{
		$set: {
			body: 'New Body of Post 2',
			category: 'Technology'
		}
	}
);

db.posts.update({ title: 'Post One' }, { $inc: { likes: 2 } });
```
> Remove
```
db.posts.remove({ title: 'Post Four' });
```
## Mongoose:

    It is a npm package to connect your express app to mongodb.
    Mongoose provides a straight-forward, schema-based solution to model your application data.

    npm install mongoose --save.
	
```javascript
    const mongoose = require('mongoose');
    mongoose.connect('mongodb://localhost:27017/database', { useNewUrlParser: true, useUnifiedTopology: true });
    const Schema = mongoose.Schema;
    const blog = new Schema({
		author: String,
		title: String,
		body: String,
		date: { type: Date, default: Date.now }
	});
```