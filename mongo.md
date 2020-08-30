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
use testdb
db
show dbs
show collections
db.dropDatabase()
db.createCollection(collection_name, { capped : true, size : 9232768, max: 10, autoIndexId:false} )
db.collection_name.insert({
  name: "balakarthikeyan",
  age: 30,
  website: "balakarthikeyan.com"
})
db.collection_name.find()
db.collection_name.drop()
db.collection_name.find().forEach(printjson) / db.collection_name.find().pretty()
db.collection_name.find({"field_name":{$gt:criteria_value}}).pretty()
db.collection_name.find({"field_name":{$lt:criteria_value}}).pretty()
db.collection_name.find({"field_name":{$ne:criteria_value}}).pretty()
db.collection_name.update({"name":"Jon Snow"},{$set:{"name":"Kit Harington"}})
db.collection_name.save( {_id:ObjectId(), new_document} )
db.collection_name.remove(delete_criteria, noOfRecord)
db.collection_name.remove({}) -- For all records
db.collection_name.find({}, {"_id": 0, "student_id": 1}) -- Projection it show only student_id
db.collection_name.find({student_id : {$gt:2002}}).limit(1).pretty()
db.collection_name.find({student_id : {$gt:2002}}).limit(1).skip(1).pretty() --Skip first record and show remaining
db.collecttion_name.find().sort({field_key:1 or -1})
db.collection_name.createIndex({field_name: 1 or -1})
db.collection_name.getIndexes()
db.collection_name.dropIndex({index_name: 1})
db.collection_name.dropIndexes()
```
