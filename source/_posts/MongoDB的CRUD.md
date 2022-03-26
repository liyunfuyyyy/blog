---
title: MongoDB的CRUD
date: 2022-03-05 10:54:11
categories: 实战
tags: ['MongoDB','数据库','Mongoose']
---

## MongoDB初见



![image-20220107133258717](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/202201071332826.png)

### Docker中MongoDB数据的备份与恢复

```shell
#备份
docker exec -it 镜像名 mongodump -h 地址 -u root -p example -o 备份到的地址
docker exec -it some-mongo mongodump -h localhost -u root -p example -o /temp/test
```



### 是什么

- 存储`文档`的`非关系型`数据库

![image-20220102093831863](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/202201020938923.png)

- 可以将多个不同的内容添加到一个集合里面，如果想要添加字段，直接添加，不会报错



## MongoDB的CRUD

### 创建文档

#### 创建一个文档

- 自己提供文档主键`_id`值，容易出现错误，可以省略文档`_id`字段，让它自动生成，`collection`为集合

```shell
db.collection.insertOne(
	{
	_id: "account1",
	name: "alice",
	balance: 100
	}
)
```

```shell
db.collection.insertOne(
	{
	name: "alice",
	balance: 100
	}
)
```

#### 创建多个文档

- `ordered`参数用来决定mongoDB是否要按顺序来写入这些文档
  - `ordered:false` 表示可以打乱文档写入顺序，以便优化写入的操作
  - `ordered:true(默认值)`按顺序执行，如果第一条插入数据错误，那么第二天不会执行

```shell
db.accounts.insertMany(
	[
		{
		name: "alice1",
		balance: 100
		},
		{
		name: "alice2",
		balance: 200
		}
	]	,
	{
		ordered:false  //可选
	}
)
```

![image-20220102095450876](https://raw.githubusercontent.com/liyunfuyyyy/img-url/master/202201020954929.png)

#### 创建单个或多个文档

- `save`命令运行时调用`insert`   `db.collection.save` 

```shell
db.accounts.insert(
	{
	name: "alice1",
	balance: 100
	},
)

db.accounts.insert(
	[
		{
		name: "alice1",
		balance: 100
		},
		{
		name: "alice2",
		balance: 200
		}
	]	
)
```



#### `insertOne` 、`insertMany` 、`insert`的区别

- 正确和错误返回的结果不同

- `insertOne`和`insertMany`命令不支持`db.collection.explain()`名
- `insert`支持`db.collection.explain`命令



### 读取文档

#### 读取全部文档

- 既不筛选，也不投射

  ```shell
  db.accounts.find()
  ```

- 更清晰的显示文档

  ```shell
  db.accounts.find().pretty()
  ```

#### 匹配查询

- 读取alice的银行账户文档

  ```shell
  db.accounts.find({name: "alice"})
  ```

- 读取alice的余额为100元的银行账户文档

  ```shell
  db.accounts.find({name: "alice",balance: 100})
  ```

- 读取复合主键的文档

  ```shell
  db.accounts.find({"_id.type": "savings"})
  ```

  

#### 比较操作符

| 操作符 | 含义                       |
| ------ | -------------------------- |
| $eq    | **相等**查询值的文档       |
| $ne    | **不等**查询值的文档       |
| $gt    | **大于**查询值的文档       |
| $gte   | **大于或等于**查询值的文档 |
| $lt    | **小于**查询值的文档       |
| $lte   | **小于或等于**查询值的文档 |
| $in    | 与任一查询值相等的文档     |
| $nin   | 与任何查询值都不等的文档   |

- 读取不属于alice的银行账户文档

  ```shell
  db.accounts.find({name: {$ne:"alice"}})
  ```

- 读取余额大于500的银行账户文档

  ```shell
  db.accounts.find({balance: {$gt:500}})
  ```

- 读取用户名字排在fred之前的银行账户文档

  ```shell
  db.accounts.find({name: {$lt:"fred"}})
  ```

- 读取alice和charlie的银行账户文档

  ```shell
  db.accounts.find({name: {$in:["alice","charlie"]}})
  ```

- 读取既不是alice和charlie的银行账户文档

  ```shell
  db.accounts.find({name: {$nin:["alice","charlie"]}})
  ```



#### 逻辑操作符

| 逻辑操作符 | 含义                         |
| ---------- | ---------------------------- |
| $not       | 筛选条件不成立的文档         |
| $and       | 多个条件全部成立的文档       |
| $or        | 至少一个筛选条件成立的文档   |
| $nor       | 多个筛选条件全部不成立的文档 |

- 读取余额不小于500的银行账户文档  

  ```shell
  db.accounts.find({balance:{$not:{$lt:500}}})
  ```

- 读取余额大于100并且用户姓名排在fred之后的银行账户文档

  ```shell
  db.accounts.find({$and:[{balance:{$gt:100}},{name:{$gt:"fred"}}]})
  ```

- 读取余额大于100并且小于500的银行账户文档

  ```shell
  db.accounts.find({balance:{$lt:500,$gt:100}}})
  ```

- 读取属于alice或者charlie的银行账户文档

  ```shell
  db.accounts.find({
  	$or:[
  		{name:{$eq:"alice"}},
  		{name:{$eq:"charlie"}}
  	]
  })
  ```

- 读取既不属于alice和charlie且余额不小于100的银行账户文档

  ```shell
  db.accounts.find({
  	$nor:[
  		{name:"alice"},
  		{name:"charlie"},
  		{balance:{$lt:100}}
  	]
  })
  ```



#### 字段操作符

| 操作符  | 含义                     |
| ------- | ------------------------ |
| $exists | 包含查询字段的文档       |
| $type   | 字段类型符合查询值的文档 |

- 读取包含账户类型字段的银行账户文档

  ```shell
  db.accounts.find({"_id.type":{$exists:true}})
  ```

- 读取文档主键是字符串的银行账户文档

  ```shell
  db.accounts.find({_id:{$type:"string"}})
  ```



#### 数组操作符

| 操作符     | 含义                                       |
| ---------- | ------------------------------------------ |
| $all       | 数组字段中包含所有查询值的文档             |
| $elemMatch | 数组字段中至少存在一个值满足筛选条件的文档 |

- 读取联系地址位于中国北京的银行账户文档

  ```shell
  db.accounts.find({contact:{$all:["china","beijing"]}})
  ```

- 读取联系电话范围在100000和200000之间的银行账户文档

  ```shell
  db.accounts.find({contact:{$elemMatch:{$gt:"100000",$lt:"200000"}}})
  ```

  

  

#### 正则操作符

- 读取用户姓名以c或者j开头的银行账号文档

  ```shell
  db.accounts.find({name:{$in:[/^c/,/^j/]}})
  ```

- 读取用户姓名包含LIE(不区分大小写)的银行账户文档

  ```shell
  db.accounts.find({name:{$regex:/LIE/,$options:'i'}})
  ```



### 文档游标

- 查询语句默认返回的是文档游标，默认只显示前二十条

#### 游标函数

```js
var cursor=db.accounts.find()
```

- `cursor.hasNext()` `cursor.next()`

- ```js
  var myCursor=db.accounts.find({name:"alice"})
  while(myCursor.hasNext()){
    printjson(myCursor.next())
  }  //只要还有就把剩余文档打印出来
  ```

- `cursor.forEach()`

- ```js
  var myCursor=db.accounts.find({name:"alice"})
  myCursor.forEach(printjson)   //每篇文档被打印
  ```

- `cursor.limit()`

- `cursor.skip()`

- ```js
  db.accounts.find({name:"alice"}).limit(1)  //只返回第一篇文档
  db.accounts.find({name:"alice"}).skip(1)   //跳过第一篇 只显示第二篇和第三篇
  ```

- `cursor.count()`

- ```js
  db.accounts.find({name:"alice"}).limit(1).count()   //返回3
  db.accounts.find({name:"alice"}).limit(1).count(true)  //返回1
  //默认不接收limit和skip返回的结果
  ```

- `cursor.sort()`

  - 按照余额从大到小，用户名按字母顺序排序

  - ```js
    db.accounts.find().sort({balance:-1,name:1})
    ```



#### 游标注意事件

- `cursor.skip()`在`cursor.limit()`之前执行
- `cursor.sort()`在`cursor.skip()`和`cursor.limit()`之前执行



#### 文档投影

- 只返回银行账户文档中的用户姓名

- ```js
  db.accounts.find({},{name:1})
  ```

- 只返回银行账户文档中的用户姓名(不包含文档主键)

- ```js
  db.accounts.find({},{name:1,_id:0})
  ```

- <mark>除了文档主键之外，我们不可以在投影文档中混合使用包含和不包含这两种投影操作要么在投影文档中列出所有应该包含的字段，要么列出所有不应该包含的字段</mark>



### 更新文档





  

  
