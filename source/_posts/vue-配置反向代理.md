---
title: vue 配置反向代理
date: 2019-05-23 22:28:32
tags:
---
vue项目中，前端调用后台接口是会遇到跨域问题，可以用jsonp或者CROS（跨域资源共享）的方式解决：

1、CROS(跨域资源共享) 后台更改header
<pre>
header(‘Access-Control-Allow-Origin:*’);//允许所有来源访问
header(‘Access-Control-Allow-Method:POST,GET’);//允许访问的方式 
</pre>　　
这样就可以跨域请求数据了。

2、使用http-proxy-middleware 代理解决（项目使用vue-cli脚手架搭建）

打开config里的index.js文件

然后更改dev里的内容
<pre>
proxyTable: {
    '/api': {
      target: 'http://www.qdd.com ',//后台提供的接口地址
      changeOrigin: true,
    }
}
</pre>
顺便说一下可以让在局域网内的所有电脑打开你的程序的方式，还是打开config里边的index.js文件，在dev里更改host为0.0.0.0
<pre>
host: '0.0.0.0',
</pre>
如有错误或其他问题可以直接邮件联系libeibei0804@outlook.com

