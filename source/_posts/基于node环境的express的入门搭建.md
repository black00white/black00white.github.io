---
title: 基于node环境的express的入门搭建
date: 2019-10-23 11:34:43
tags:
---
最近打算自己搭建一个简单的后台，来为自己的前端代码提供基础的接口，学过一些node.js的基础，首先就想到了基于node的express框架，记录下首次搭建的过程。

首先确认电脑上安装了node环境，在命令行工具敲node -v，如果显示除了版本号，则已经安装了node环境。如果没有安装，点击下载链接 [http://nodejs.cn/download/](http://nodejs.cn/download/)，选择和你电脑系统相匹配的版本下载即可。
```
node -v
v10.16.0
```

<font color=#409EFF face="微软雅黑">安装express</font>

新建一个文件夹，然后在文件夹内执行npm init为应用程序创建 package.json文件，全部回车默认执行即可。
```
npm init
```

然后安装express,并将其保存在依赖项列表中。
```
npm install express --save
```

<font color=#409EFF face="微软雅黑">初始化express服务</font>

创建一个名为server的文件夹，在里边创建一个app.js，在里边添加如下代码：
```
var express = require('express')
var app = express()

app.get('/',function(req,res){
    res.send('hello world')
})

app.listen(3000, function(){
    console.log('hello')
})
```

使用以下命令运行程序
```
node app.js
```

然后在浏览器输入http://localhost:3000/就可以看到刚启动的服务了。

附express中文网链接[https://expressjs.com/zh-cn/starter/installing.html](https://expressjs.com/zh-cn/starter/installing.html)。

