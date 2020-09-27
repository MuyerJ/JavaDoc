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

# 修改器
```xml
修改器：
对于MongDB数据库而言，数据的修改会牵扯到内容的变更、结构的变更(包含有数组)，所以在进行MongoDB设计的时候，就提供有一系列修改器的应用，那么之前使用的"$set"就是一个修改器的使用
1、$inc:主要针对于一个数字字段，增加某个数字字段的内容；
语法：{"$inc":{成员：内容}}
范例：将所有年龄为19岁的学生成绩一律减少30分,年龄加一岁
db.students.update({"age":19},{"$inc":{"score":-30,"age":1}},false,true);//全部执行，false,true
2、$set:进行内容的重新设置
语法：{"$set":{"成员"："新内容"}}
范例：将年龄是20岁的人的成绩修改为89分
db.students.update({"age":20},{"$set":{"score":19}})
3、$unset:删除某个成员的内容
语法：{"$unset":{"成员":1}}
范例：删除"张三"的年龄与成绩信息
db.students.update({"name":"张三"},{"$unset":{"age":1,"score":1}})
执行之后指定的成员内容就消失了。
4、$push:相当于将内容追加到指定的成员之中（基本上是数组)
语法：${"$push":{"成员"：value}}
范例：向张三添加课程信息（此时张三信息里面没有course信息）
db.students.update({"name":"张三"},{"$push":{"course":["语文","数学"]}})
范例：向"谷大声-E"里面的课程追加一个"美术"
db.students.update({"name":"股大声-E"},{"$push":{"course":"美术"]}})
push就是进行数组数据的添加操作使用，如果没有数组则进行一个新的数组的创建，如果有则进行内容的追加。
5、$pushAll:与"$push"类似的，可以一次追加多个内容到数组里面：
语法：${"$pushAll":{成员：数组内容}}
范例：向"王五"的信息里面添加多个课程内容
db.students.update({"name":"王五"},{"$pushAll":{"course":["语文","数学"]}})
6、$addToSet:向数组里面添加一个新的内容，只有这个内容不存在的时候才会增加。
语法：{"$addToSet":{成员：内容}}
范例：向王五的信息增加新的内容
db.students.update({"name":"王五"},{"$addToSet":{"course":"跳舞"}})
此时会判断要增加的内容在数组里面是否已经存在了，如果不存在则追加内容，如果存在，则不增加内容。
7、$pop:删除数组内的数据：
语法：{"$pop":{成员：内容}}，内容如果设置为-1表示删除第一个，1表示删除最后一个
范例：删除王五的一个课程
db.students.update({"name":"王五"},{"$pop":{"course":"-1"}})
范例：删除王五的最后一个课程：
db.students.update({"name":"王五"},{"$pop":{"course":"1"}})
8、$pull：从数组内删除一个指定内容的数据
语法：{"$pull",{成员：数据}} 进行数据比对，如果是此数据则进行删除
范例：删除王五内的跳舞的课程，如果存在就删除
db.students.update({"name":"王五"},{"$pull":{"course":"跳舞"}})
9、$pullAll:一次性删除多个内容
语法：{"$pull":{成员：[数据1,数据2]}}
范例：删除”顾大神-a“的二门课程
db.students.update({"name":"股大声"},{"$pullAll":{"course":["跳舞","音乐"]}})
10、$rename:为成员名称重命名
语法：{"$rename":{旧的成员名称：新的成员名称}}
范例：将"张三"name成员名称修改为"姓名"
db.students.update({"name":"王五"},{"$rename":{"name":"姓名"}})
在整个的MongoDB数据库里面，提供的修改器的支持很到位。
```