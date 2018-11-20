---
title: mongoDB的使用
tags: MongoDB,数据库
categories: MongoDB
archives: 前端技术
date: 2018-10-25 22:46:08
photos: 
    - "http://oz2tkq0zj.bkt.clouddn.com/17-11-9/52323298.jpg"
---

##  什么是MongoDB？

1.  MongoDB 是由C++语言编写的，是一个基于分布式文件存储的开源数据库系统。
2.  在高负载的情况下，添加更多的节点，可以保证服务器性能。
3.  MongoDB 旨在为WEB应用提供可扩展的高性能数据存储解决方案。
4.  MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。


##  MongoDB的安装

具体细节请参照[该网站自行配置](http://www.runoob.com/mongodb/mongodb-window-install.html)

##  MongoDB概念解析

**MongoDB的存储结构**和以前我们的关系型数据库的数据结构都是顶层是库，库下面是表，表下面是数据。但是MongoDB有所不同，库下面是集合，集合下面是文件

##  基础Shell命令

1.  show dbs :显示已有数据库，如果你刚安装好，会默认有local、admin(config)，这是MongoDB的默认数据库，我们在新建库时是不允许起这些名称的。
2.  use admin： 进入数据，也可以理解成为使用数据库。成功会显示：switched to db admin。
3.  show collections: 显示数据库中的集合（关系型中叫表，我们要逐渐熟悉）。
4.  db:显示当前位置，也就是你当前使用的数据库名称，这个命令算是最常用的，因为你在作任何操作的时候都要先查看一下自己所在的库，以免造成操作错误。

##  数据操作基础命令

1.  use db（建立数据库）：use不仅可以进入一个数据库，如果你敲入的库不存在，它还可以帮你建立一个库。但是在没有集合前，它还是默认为空。
2.  db.集合.insert( ):新建数据集合和插入文件（数据），当集合没有时，这时候就可以新建一个集合，并向里边插入数据。Demo：db.user.insert({“name”:”jspang”})
3.  db.集合.find( ):查询所有数据，这条命令会列出集合下的所有数据，可以看到MongoDB是自动给我们加入了索引值的。Demo：db.user.find()
4.  db.集合.findOne( ):查询第一个文件数据，这里需要注意的，所有MongoDB的组合单词都使用首字母小写的驼峰式写法。
5.  db.集合.update({查询},{修改}):修改文件数据，第一个是查询条件，第二个是要修改成的值。这里注意的是可以多加文件数据项的，比如下面的例子。
6.  db.集合.remove(条件)：删除文件数据，注意的是要跟一个条件。Demo:db.user.remove({“name”:”jspang”})
7.  db.集合.drop( ):删除整个集合，这个在实际工作中一定要谨慎使用，如果是程序，一定要二次确认。
8.  db.dropDatabase( ):删除整个数据库，在删除库时，一定要先进入数据库，然后再删除。实际工作中这个基本不用，实际工作可定需要保留数据和痕迹的。

##  mongoose的使用
**Mongoose是一个开源的封装好的实现Node和MongoDB数据通讯的数据建模库。**

### mongoose的安装
`npm i mongoose -S`

### 连接数据库
在`database`目录下新建一个`init.js`文件。
```
const mongoose = require('mongoose')
const db = "mongodb://localhost/smile-db"
exports.connect = ()=>{
  //连接数据库
  mongoose.connect(db)

  let maxConnectTimes = 0 
  return  new Promise((resolve,reject)=>{
    //把所有连接放到这里
    //增加数据库监听事件
    mongoose.connection.on('disconnected',()=>{
      console.log('***********数据库断开***********')
        if(maxConnectTimes<3){
          maxConnectTimes++
          mongoose.connect(db)    
        }else{
          reject()
          throw new Error('数据库出现问题，程序无法搞定，请人为修理......')
        }
      })
    mongoose.connection.on('error',err=>{
      console.log('***********数据库错误***********')
      if(maxConnectTimes<3){
        maxConnectTimes++
        mongoose.connect(db)   
      }else{
        reject(err)
          throw new Error('数据库出现问题，程序无法搞定，请人为修理......')
        }
    })
    //链接打开的时
    mongoose.connection.once('open',()=>{
      console.log('MongoDB connected successfully') 
      resolve()   
    })
  })
}
```

### Mongoose的Schema建模
**Schema是以key-value形式Json格式的数据。**

**Schema中的数据类型**
* String ：字符串类型
* Number ：数字类型
* Date ： 日期类型
* Boolean： 布尔类型
* Buffer ： NodeJS buffer 类型
* ObjectID ： 主键,一种特殊而且非常重要的类型
* Mixed ：混合类型
* Array ：集合类型

####  Mongoose中的三个概念
1.  schema ：用来定义表的模版，实现和MongoDB数据库的映射。用来实现每个字段的类型，长度，映射的字段，不具备表的操作能力。
2.  model ：具备某张表操作能力的一个集合，是mongoose的核心能力。我们说的模型就是这个Mondel。
3.  entity ：类似记录，由Model创建的实体，也具有影响数据库的操作能力。

####  初学定义一个Schema
我们在`database`文件夹下新建一个`Schema`文件下，然后创建一个`use.js`文件
```
const mongoose = require('mongoose')    //引入Mongoose
const Schema = mongoose.Schema          //声明Schema
let ObjectId = Schema.Types.ObjectId    //声明Object类型
//创建我们的用户Schema
const userSchema = new Schema({
    UserId:ObjectId,
    userName:{unique:true,type:String},
    password:String,
    createAt:{type:Date,default:Date.now()},
    lastLoginAt:{type:Date,default:Date.now()}
})
//发布模型
mongoose.model('User',userSchema)
```

##  载入Schema和插入查出数据
我们在`init.js`文件中处理**Schema**。首先我们载入所有的`Schema`，然后再处理。
安装*glob*
`npm i glob -S`
```
const glob = require('glob')
const {resolve} = require('path') //将一系列相对路径替换成绝对路径
```
```
exports.initSchemas = () =>{
    glob.sync(resolve(__dirname,'./schema/','**/*.js')).forEach(require)
}
```

```
const Koa = require('koa')
const app = new Koa()
const mongoose = require('mongoose')
const {connect , initSchemas} = require('./database/init.js')
//立即执行函数
;(async () =>{
    await connect()
    initSchemas()
    const User = mongoose.model('User')
    let oneUser = new User({userName:'jspang13',password:'123456'})
    oneUser.save().then(()=>{
        console.log('插入成功')
    })
let  users = await  User.findOne({}).exec()
console.log('------------------')
console.log(users)
console.log('------------------')  
})()
app.use(async(ctx)=>{
    ctx.body = '<h1>hello Koa2</h1>'
})
app.listen(3000,()=>{
    console.log('[Server] starting at port 3000')
})
```