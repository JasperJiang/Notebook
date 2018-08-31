# Mongo Common



启动mongo

```bash
mongod --config /usr/local/etc/mongod.conf
```

进入mongo命令行

```bash
mongo
```

选择数据库

```bash
use database_name
```

删除数据库

```bash
db.dropDatabase()
```

插入数据

```bash
var user1 = {name:”user1”,passworld:”test123”}
db.user.insert(user1)
```

删除数据

```bash
db.user.remove()
```

查找数据

```bash
db.collection.find(query, projection)

$or: [
          {key1: value1}, {key2:value2}
      ]
```

| 说明 | 语法 | 例子 | 对应SQL |
| :--- | :--- | :--- | :--- |
| 等于 | `{<key>:<value>`} | `db.col.find({"by”:"sss").pretty()` | `where by = 'sss'` |
| 小于 | `{<key>:{$lt:<value>}}` | `db.col.find({"likes":{$lt:50}}).pretty()` | `where likes < 50` |
| 小于或等于 | `{<key>:{$lte:<value>}}` | `db.col.find({"likes":{$lte:50}}).pretty()` | `where likes <= 50` |
| 大于 | `{<key>:{$gt:<value>}}` | `db.col.find({"likes":{$gt:50}}).pretty()` | `where likes > 50` |
| 大于或等于 | `{<key>:{$gte:<value>}}` | `db.col.find({"likes":{$gte:50}}).pretty()` | `where likes >= 50` |
| 不等于 | `{<key>:{$ne:<value>}}` | `db.col.find({"likes":{$ne:50}}).pretty()` | `where likes != 50` |

```bash
db.col.find({"title" : {$type : 2}})
```

title 中 type为string

修改数据

```bash
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)

db.collection.update({"id":sadsad},{"$set":{"name":"user1"}})
```

其他操作

```text
db.collection.aggregate(pipeline, options)
$project：包含、排除、重命名和显示字段
$match：查询，需要同find()一样的参数
$limit：限制结果数量
$skip：忽略结果的数量
$sort：按照给定的字段排序结果
$group：按照给定表达式组合结果
$unwind：分割嵌入数组到自己顶层文件
```

```bash
for(i=1;i<100;i++)db.test1.insert({x:i})
db.test1.find({x:1})
db.test1.update({x:1},{$set:{y:1}})
```

 set字符保证只修改需要修改的，如果不加则全部替换



更新不存在的数据

```bash
db.test1.update({y:2},{y:3},true)
```

true表示如果更新的时候数据不存在则自动添加这条数据

注：默认情况下update只更新第一条找到的数据



如果要多跳数据则

```bash
db.test1.update({y:2},{$set:{y:3}},false,true) 
```

必须要用set来替换，并且不自动添加



删除数据操作

```bash
db.test1.remove()
```

删除表

```bash
db.test1.drop()
```

查看索引

```bash
db.test1.getIndexes()
```

创建索引

```bash
db.test1.ensureIndex({x:1})
```

x=1代表正向，x=-1代表逆向

