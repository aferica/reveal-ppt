## 快速搭建自己的API服务器和网站

---



### 1.Node服务器搭建

使用Node + Express + Mongodb快速搭建API服务器

---



#### 1.1 项目简单目录结构介绍
``` js
.		#项目总目录
├── config		#项目配置文件夹
│	├── db_config.js		#数据库配置文件
├── models		#逻辑模块文件夹
│	├── user		#user模块文件夹
│	│	├── user_controller.js		#user逻辑实现文件
│	│	├── user_model.js		#user模型文件
|
├── route.js		#程序路由文件夹
│ ├── user_route.js		#user模块路由文件
|
├── app.js		#程序入口文件
└── package.json		#Node项目依赖文件

```
---

#### 1.2 项目基本配置介绍
##### 1.2.1 package.json配置
``` json
{
  "name": "node-server",
  "version": "1.0.0",
  "description": "\"aferica的后台服务接口\"",
  "main": "main.js",
  "scripts": {
    "start": "supervisor main"
  },
  "author": "aferica",
  "license": "MIT",
  "dependencies": {
    "依赖名": "依赖版本"
  },
  "devDependencies": {
    "supervisor": "^0.12.0"
  }
}

```
---

##### 1.2.2 db_config.js配置
``` js
var mongoose = require("mongoose");
var promise = require('bluebird');

// Build the connection string
var dbURI = 'mongodb://127.0.0.1:27017/data';

mongoose.Promise = promise;
// Create the database connection
mongoose.connect(dbURI,{useMongoClient:true});

// CONNECTION EVENTS
// When successfully connected
mongoose.connection.on('connected', function () {
    console.log('Mongoose default connection open to ' + dbURI);
});

// If the connection throws an error
mongoose.connection.on('error',function (err) {
    console.log('Mongoose default connection error: ' + err);
});

// When the connection is disconnected
mongoose.connection.on('disconnected', function () {
    console.log('Mongoose default connection disconnected');
});

// If the Node process ends, close the Mongoose connection
process.on('SIGINT', function() {
    mongoose.connection.close(function () {
        console.log('Mongoose default connection disconnected through app termination');
        process.exit(0);
    });
});

exports.dbURI = dbURI;
```
---
#### 1.3 项目模块实现介绍
---

##### 1.3.1 项目流程介绍
``` sequence
participant app.js
participant route.js
participant controller.js
participant mongodb

```
---

##### 1.3.2 app.js配置
``` js
const express = require("express"); 
const path = require("path"); 
const bodyParser = require("body-parser"); 
const compress = require('compression'); 
const logger = require('morgan'); 
const crypto = require("crypto"); 
const mongoose = require('mongoose'); 
const session = require('express-session'); 
const MongoStore = require('connect-mongo')(session); 
const moment = require('moment'); 
const dbURI = require('./config/db_config').dbURI;
mongoose.createConnection(dbURI); //连接数据库

const app = express();

app.disable('x-powered-by');
app.use(logger('dev'));

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: false }));
app.use(function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Credentials", "true");
    res.header("Access-Control-Allow-Methods", "*");
    res.header("Access-Control-Allow-Headers", "Content-Type,Access-Token");
    res.header("Access-Control-Expose-Headers", "*");
    next();
});

app.use(session({
    key: 'session',
    secret: 'Keboard cat',
    cookie: {maxAge: null},
    store : new MongoStore({
        db: 'Notes',
        mongooseConnection: mongoose.connection
    }),
    resave: false,
    saveUninitialized: true
}));
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(compress());

const user = require('./routes/user_route')

user(app);

app.get('/test', function (req, res) {
    res.send('sign success');
})

app.listen(3000,function (req,reset) {
    console.log('app is running at port 3000');
});

```
---

##### 1.3.3 route.js实现




#### 2.Angular2网站快速实现

---



#### 3.服务器Nginx部署

---



#### 4.常用工具推荐

---


