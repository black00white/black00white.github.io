---
title: 对接友盟统计+网站统计之vue单页面嵌入
date: 2019-11-16 10:44:48
tags:
---
网站统计对接友盟，官网上已经有相关demo，具体的步骤可[点击这里](https://help.cnzz.com/support/kuaisuanzhuangdaima/2013/0829/7.html)查看。现在要说的是，大部分的前端开发已经摒弃了jquery的方式，在用vue或者react等单页的方式开发，而友盟监测不到单页面路由的变化，这时候就需要做一些改造。

如果你是需要全网站监测，那么可在app.vue中直接引用，如果是监测部分页面，只需在所需要的页面引入以下代码即可
```
watch: {
    '$route' () {
        if (window._czc) {
            //监听路由变化
            let location = window.location;
            let contentUrl = location.pathname +location.hash;
            let refererUrl = '/';
            window._czc.push(['_trackPageview',contentUrl, refererUrl])
            console.log(location,contentUrlrefererUrl)
        }
    },
},
mounted(){
    //友盟
    const script = document.createElemen('script')
    script.src = "https://s4.cnzz.com/z_stat.phpid=yourId&web_id=yourId"//友盟链接
    script.language = 'JavaScript'
    document.body.appendChild(script)
}
```

谁知之后在Chrome浏览器上测试，友盟一直没有统计到数据，在页面上打印window._czc，打印出来是undefined，网上搜了资料才发现是Chrome的一些插件会把友盟的js给屏蔽掉，然后换了其他浏览器，打开控制台的network已经看到友盟的js在运行，再次打开幽梦也已经有了统计数据了。

这个已经足以监测到最简单的pv、uv了，如果需要事件监测、下载监测等就需要自己对接友盟的api了，等我用到了再来分享吧。