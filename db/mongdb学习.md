参考文档：https://www.yiibai.com/mongodb/mongodb_drop_collection.html
# cmd中启动mongodb

###### -mongo

---

# 配置文件（mongo.conf）
###### 在bin同级目录下创建一个配置文件mongo.conf
- dbpath=F:\mongodb\data #数据库路径
- logpath=F:\mongodb\logs\mongo.log #日志输出文件路径
- logappend=true #错误日志采用追加模式
- journal=true #启用日志文件，默认启用
- quiet=true #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
- port=27017 #端口号 默认为27017

---

# 数据库

### 创建
use dbname

### 查看
show dbs

### 删除
db.dropDatabase() 需要先切换到你想删除的数据库

---

# 集合
### 创建集合
db.createCollection('')
### 删除集合
db.collection.drop()

---

# 文档
### 插入
###### db.collection.insert()
###### db.collection.insertOne()
###### db.collection.insertMany()
### 更新
###### db.collection.update( criteria, objNew, upsert, multi )

- criteria :查询条件
- objNew：更新的数据
- upsert：不存在的是否插入
- multi：多条是否更改


### 删除
###### db.collection.remove(<query>,<justOne>)

- query：删除文档的条件
- justOne:删除是否是一个，如果设置为true或1，则只删除一个文档。


### 查询
###### find() findOne()
###### pretty()变得好看
###### find({},{KEY:1})的第二个参数 KEY,1对于字段显示，0对应字段不显示
###### limit()
###### sort(Key:1),KEY,1用于升序，-1用于降序
###### 一些符号：
- {"":""}
- {"":{$lt/lte/gt/gte/ne:}}
- {"":""，"":""}
- $OR:[] 或者 
- ,(AND) 和

备注：and和or冒号后面跟一个对象数组
例子：
```
db.col.find({$or:[{"score":"11"},{"score":"11111"}]})
db.col.find({"score":"11",$or:[{"score":"11111"}]})
```
- 大于和小于连用 
    
```
db.col.find({likes : {$lt :200, $gt : 100}})
```
- $type
获取 "col" 集合中 title 为 String 的数据

```
db.col.find({"title" : {$type : 2}})
```
### 索引
###### db.COLLECTION_NAME.ensureIndex({KEY:1})，KEY，1用于升序，-1用于降序



---
# 训练：
###### URL：https://www.cnblogs.com/my-blogs-for-everone/articles/9750755.html
```
//删除所有数据不删除集合
db.col.remove({})
//查找、删除表中不存在某个字段的数据 {"school": {$exists:false} }
db.col.remove({"school":{$exists:false}})
//更新　不加multi只更新一条
//普通更新
//条件更新　{$max:{age:updateValue}} {$unset:{school:true}} 
db.col.update({name:"张三"},{$max:{age:19}})
db.col.update({},{$max:{age:40}},{multi:true})
db.col.update({name:"李四"},{$unset:{school:true}})
db.col.update({},{$unset:{school:true,age:true}},{multi:true})
//多条数据共同增长：　{$inc:{age:1}},{multi:true}
db.col.update({},{$inc:{gpa:100}},{multi:true})
//有就更新，无就插入{upsert:true}
db.col.update({name:"yejiang"},{"age":100},{upsert:true})

//查找　
//查找没有某个字段的数据
//$or $and + 数组
//判断大于、小于、不等于
//模糊查询
```

