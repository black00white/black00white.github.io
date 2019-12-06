---
title: jsonwebtoken生成token，express-jwt实现Token验证和解析
date: 2019-12-06 17:06:00
tags:
---
token登录是前端登录最常用的方法，jwt提供了多种后台语言生成token的方法，本次主要实践的是node中express框架的生成和校验方法。

首先在express项目中安装jsonwebtoken和express-jwt，其中jsonwebtoken主要是用于生成token，express-jwt作为封装好的中间件用于解析和验证token
```
npm install jsonwebtoken --save
npm install express-jwt --save
```
在server目录下新增token\constant.js和token\index.js

Token验证的配置文件放在index.js
```
const expressJwt = require('express-jwt');
const { secretKey } = require('./constant');

const jwtAuth = expressJwt({secret: secretKey}).unless({path:['/users/login']})

//unless 为排除那些接口,不验证Token,这里排除 '/users/login'

module.exports = jwtAuth;
```
公共配置放在contant.js
```
const crypto = require('crypto');

module.exports = {
  MD5_SUFFIX: '805696667',
  md5: (pwd) => {
    let md5 = crypto.createHash('md5');
    return md5.update(pwd).digest('hex');
  },
  secretKey: 'secret12345'  //Token加密公共部分
};
```
在实际的使用的业务代码中，博主是在登录的时候用到的加密，写在user.js中
```
var express = require("express");
var router = express.Router();
var jwt = require("jsonwebtoken");
var User = require("./../models/users");
const { secretKey } = require("./../token/constant"); //提取Token加密内容
// // 全局验证Token是否合法
const tokens = require("./../token/index"); //验证Token配置文件
// //注册token配置文件
router.use(tokens);
let tokenKey = secretKey; //加密内容

router.post("/login", function(req, res, next) {
    var params = {
        userName: req.body.userName
        // password: req.body.password
    };
    User.findOne(params, (err, doc) => {
        // console.log(err, doc, params);
        if (err) {
            res.json({
                status: "0",
                msg: err.message
            });
        } else {
            if (doc) {
                //存在该账号
                if (doc.password == req.body.password) {
                    //验证密码是否正确
                    let tokenObj = {//需要加密的数据
                        userName: doc.userName,
                        password: doc.password
                    };
                    let token = jwt.sign(tokenObj, tokenKey, {
                        expiresIn: 60 * 60 * 24 // token时长
                    });
                    res.json({
                        status: "1",
                        msg: "success",
                        data: {
                            token: token,
                            userName: doc.userName
                        }
                    });
                } else {
                    res.json({
                        status: "0",
                        msg: "密码错误"
                    });
                }
            }
        }
    });
});


```
到此后端就结束了，保存重启服务就用postman直接调用接口就已经成功了，但是在前端的axios的请求拦截的请求头中加上Authorization之后，再去请求接口一直报401未登录，尝试了很多办法，最后发现用jwt生成的token在请求头中传给后台的时候需要加上一段字符串：'Bearer '，注意Bearer后边一定要跟一个英文的空格，目前博主还没有深究这个问题的原因，只是实现了这个功能，待以后研究原理后再行解释。
```
config => {
    if (localStorage.token) { //判断token是否存在
        config.headers.Authorization = 'Bearer ' + localStorage.token;  //将token设置成请求头
    }
    return config;
},
```
参考文档：[https://juejin.im/post/5dad720ff265da5bbb1e5571](https://juejin.im/post/5dad720ff265da5bbb1e5571)

[https://juejin.im/post/5d146767f265da1bac402976](https://juejin.im/post/5d146767f265da1bac402976)