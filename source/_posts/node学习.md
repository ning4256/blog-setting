---
title: node学习
date: 2018-10-24 18:42:47
tags: node
categories: node
archives: 前端技术
---

##  Node.js是什么？
1.  Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。 
2.  Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型，使其轻量又高效。


##  Node.js的安装和配置
这个在这里不过多阐述，更多内容请自行百度。

##  第一个Node.js文件
在一个目录下创建一个`nodejs`的文件夹,同时在该文件夹下创建一个`HelloWorld.js`文件，其内容为
```
console.log('Hello,World!');
```
然后打开终端，进入该文件目录下，使用`node HelloWorld.js`就可以在终端看到 **Hello,World!**的字了，并且第一个nodejs文件就完成了。

##  node核心模块
node具有众多的核心模块，比较常用的有 **fs,http,path**等

### fs模块
fs是node中用于 **读取文件**的，它常用的有 **readFile()和writeFile()**这两个方法，第一个参数都是文件地址，*readFile()*第二个参数就是一个回调函数，而*writeFile()*由于是写文件，故应当有写入内容，所以其第二个参数是data(string)，然后跟一个回调函数。

readFile()方法
```
var fs = require('fs');

fs.readFile('1.txt', (error, data) => {
  if(error) {
    console.log('读取文件失败，请稍后再试')
  }
  else {
    console.log(data.toString())  //读取的数据大多是二进制数据，所以需要将其转换为字符类型
  }
})
```
同理，writeFile()方法
```
var fs = require('fs');

fs.writeFile('2.txt', '我是写入的内容', (error) => {
  if(error) {
    console.log('写入文件失败，请稍后再试')
  }
  else {
    console.log('写入文件成功')
  }
})
```
比较上面两段代码，可以清楚的知道，readFile()方法有两个参数，而回调函数中有两个参数（error和data）；而writeFile()方法有三个参数，而回调函数就只有一个参数（error）了。

### http模块
http模块主要用于搭建http服务器，我们可以利用其构建后端服务，使我们的应用在web服务器上使用。
```
var http = require('http)
var server = http.createServer()
server.on('request', (req, res) => {
  var url = res.url;
  if(url === '/') {
    fs.readFile('./index.html', (error, data) => {
    if(error) {
      res.setHeader('Content-Type', 'text/plain;charset=utf-8')
      res.end('读取文件失败，请稍后再试！')
      }
    else {
      res.setHeader('Content-Type', 'text/html;charset=utf-8')
      res.end(data)
      }
    })
  }
})
server.listen(3000, () => {
  console.log('The server is starting at port 3000!')
})
```
常用查询[Content-Type的网站](http://tool.oschina.net/)

##  node实现Apache
```
var http = require('http')
var fs = require('fs')
var server = http.createServer()

server.on('request', function(req, res) {
  var url = req.url
  var pathFile = 'index.html'
  if(pathFile !== '/') {
    pathFile = url
  }
  fs.readFile(pathFile, function(err, data) {
    if(err) {
      console.log('读取文件失败')
    }
    else {
      res.end(data)
    }
  }) 
})

server.listen(3000, function() {
  console.log('the server is starting at port 3000!')
})
```
##  express
express作为一个被node封装好的http框架，使用十分频繁和场景。
```
var express = require('express')
var app = new express()

app.get('/', function(req, res) {
  res.end('这是主页面')
})

app.listen(3000, function() {
  console.log('The server is starting at port 3000!')
})
```

### 使用express完成一个crud
1.  模版引擎的安装
`npm i art-template express-art-template -S`
在文件中使用
```
配置art-template模版引擎
app.engine('html', require('express-art-template'))
渲染模版引擎
res.render('index.html', {
  //  传入的对象通常是字符串需要通过JSON.parse()转移
})
```
2.  完整代码
```
// 引包
var express = require('express')
var fs = require('fs')

// 创建服务器
var app = new express()

// 配置公共资源
app.use('/public/', express.static('./public'))

app.engine('html', require('express-art-template'))
// 配置请求
app.get('/', function(req, res) {
  fs.readFile('./db.json', 'utf8', function(err, data) {
    if(err) {
      return res.status(500).send('error!')
    }
    res.render('index.html', {
      user: JSON.parse(data).user
    })
  })
})

// 监听请求
app.listen(3000,function() {
  console.log('running 3000')
})
```

### router路由模块
通常情况下，我们将需要请求的路径封装成一个`router.js`文件，里面封装好我们需要的路径和相应的请求回调函数和逻辑，使我们的文件逻辑清晰。
```
var express = require('express')
var router = express.Router()
var fs = require('fs')

router.get('/', function(req, res) {
  fs.readFile('./db.json', 'utf8', function(err, data) {
    if(err) {
      return res.status(500).send('error!')
    }
    res.render('index.html', {
      user: JSON.parse(data).user
    })
  })
})
    
router.get('/students', function(req, res) {

})
    
router.post('/students', function(req, res) {

})
    
router.get('/students/new', function(req, res) {

})

router.post('/students/new', function(req, res) {

})

module.exports = router
```
而在我们的`app.js`文件中就可以写成这样了
```
// 引包
var express = require('express')
var router = require('./router.js')

// 创建服务器
var app = new express()

// 配置公共资源
app.use('/public/', express.static('./public'))

// 使用路由
app.use(router)

app.engine('html', require('express-art-template'))
// 配置请求


// 监听请求
app.listen(3000,function() {
  console.log('running 3000')
})

module.exports = app
```
最后完整版
`app.js`
```
//引包
const express=require('express');
const fs=require('fs');
const bodyParser=require('body-parser');
//用户路由router
const user=require('./router/user');

//创建服务器
const app=express();

//配置公共资源
app.use(express.static('./public'));

//配置使用art-template 模版引擎
app.engine('html',require('express-art-template'));

//配置body-parser
app.use(bodyParser.urlencoded({extended:false}));
app.use(bodyParser.json());

//分发路由
app.use(user);

//监听端口
app.listen(3000,function(){
  console.log('Server is Start');
});
```
`userModel.js`
```
/**
数据操作模块只负责处理数据
*/
const fs=require('fs');

const DB='./db.json'

/*
获得所有用户
return []
*/
exports.findAllUser=function(callback){
  fs.readFile(DB,'utf-8',function(error,data){
      if (error) {
        callback(error,null);
      }else {
        callback(null,JSON.parse(data).users);
      }

  });
}
/*
根据id查找user
*/
exports.findUserById=function(id,callback){
  fs.readFile(DB,'utf-8',function(error,data){
      if (error) {
        callback(error,null);
      }else {
        //查找指定id的user
        var users=JSON.parse(data).users;
        var findUser=users.find(function(item){
                    return item.id===id;
        });
        callback(null,findUser);
      }

  });
}



/*
添加用户
*/
exports.addUser=function(user,callback){
  fs.readFile(DB,'utf-8',function(error,data){
      if (error) {
        callback(error);
      }else {
        var users=JSON.parse(data).users;
        //设置id
        user.id =+users[users.length-1].id+1;
        user.id=user.id.toString();
        //添加
        users.push(user);

        var saveString=JSON.stringify({
          users:users
        });
        //写入数据
        fs.writeFile(DB,saveString,function(error){
          if(error){
            callback(error);
          }else {
            callback(null);
          }
        });
      }

  });
}
/*
修改用户
*/
exports.updateUser=function(user,callback){
  fs.readFile(DB,'utf-8',function(error,data){
      if (error) {
        callback(error);
      }else {
        var users=JSON.parse(data).users;

        //查找指定id的user
        var users=JSON.parse(data).users;
        var findUser=users.find(function(item){
                    return item.id===user.id;
        });
        //修改
        for(var key in findUser){
          findUser[key]=user[key];
        }

        var saveString=JSON.stringify({
          users:users
        });
        //写入数据
        fs.writeFile(DB,saveString,function(error){
          if(error){
            callback(error);
          }else {
            callback(null);
          }
        });
      }

  });
}
/*
删除用户
*/
exports.DeleteUser=function(id,callback){
  fs.readFile(DB,'utf-8',function(error,data){
      if (error) {
        callback(error);
      }else {
        //查找指定id的user
        var users=JSON.parse(data).users;
        var deleteUserID=users.findIndex(function(item){
                    return item.id===id;
        });

        //删除数据
        users.splice(deleteUserID,1);
        var saveString=JSON.stringify({
          users:users
        });
        
        //写入数据
        fs.writeFile(DB,saveString,function(error){
          if(error){
            callback(error);
          }else {
            callback(null);
          }
        });
      }
  });
}
```
`router.js`
```
//路由模块 根据请求路径配置请求方法


//引包
const express=require('express');

//挂载Express的路由router
const router=express.Router();

//加载模型
const userModel=require('../model/userModel');

//设置文件数据库路径
const dbPath='./db.json';




//配置请求

//查看用户列表
router.get('/user',function(req,res){
  userModel.findAllUser(function(error,data){
      if(error){
        return res.status(500).send('Server Error!!');
      }else {
        res.render('index.html',{
          user:data
        });
      }
  });
});

//添加用户页面
router.get('/user/add',function(req,res){
res.render('useradd.html');
});

//添加用户操作
router.post('/user/add',function(req,res){
var user=req.body;
userModel.addUser(user,function(error){
  if(error){
    return res.status(500).send('Server Error!!');
  }else {
    res.redirect('/user');
  }
});

});

//更新修改页面
router.get('/user/update',function(req,res){
var id=req.query.id;
userModel.findUserById(id,function(error,data){
  if (error) {
      return res.status(500).send('Server Error!!');
  }else {
    res.render('userUpdate.html',{user:data});
  }
});

});

//更新数据
router.post('/user/update',function(req,res){
  var user=req.body;
  userModel.updateUser(user,function(error){
    if(error){
        return res.status(500).send('Server Error!!');
    }else {
      res.redirect('/user');
    }
  });
});

//删除数据
router.get('/user/Delete',function(req,res){
var id=req.query.id;
userModel.DeleteUser(id,function(error){
  if (error) {
      return res.status(500).send('Server Error!!');
  }else {
    res.redirect('/user');
  }
});

});

//导出router
module.exports=router;

```