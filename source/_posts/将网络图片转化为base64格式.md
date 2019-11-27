---
title: 将网络图片转化为base64格式
date: 2019-11-27 16:50:54
tags:
---
接上篇文章中说到因为跨域的原因，需要把从后台获取过来的图片链接转换为base64格式，这里就用到了canvas的toDataURL来转换，不多说，上代码。
```
imageToBase64(url,callback){
    var mycanvas = document.createElement("canvas")
    var ctx = mycanvas.getContext('2d')
    //此处最好是获取页面已经有的元素，以方便获取宽高，否则可能会出现获取到的宽高为0，canvas画图失败的问题
    var img = document.querySelector('.share-img')
    var data = null
    //这里需要设置crossOrigin的值，允许跨域，不然会报错，一定要在设置完之后再给src赋值
    img.setAttribute("crossOrigin",'anonymous')
    img.src = url;
    var w = parseInt(window.getComputedStyle(img).width);
    var h = parseInt(window.getComputedStyle(img).height);
    mycanvas.width = w
    mycanvas.height = h
    img.onload = function(){
        //一定要在img.onload加载完图片之后再去执行drawImage,否则会因为获取不到图片元素而报错，或者图片没有画到canvas画布上
        // 将图片画到canvas上面上去！
        ctx.drawImage(img,0,0,w,h);
        data = mycanvas.toDataURL('image/jpeg')
        //这里一定要回调一下，在回调里获取到base64的值，然后赋值给原来的图片的src
        callback.call(this, data);
    }
},

this.imageToBase64(this.img,function(url){
    //本来是在这里直接把返回的base64的值赋值给了this.img以期望页面上的图片地址替换为base64格式，奈何怎么赋值都没有实现，使用了vue的深度监听，也没有监听到img值的变化，至今不知道是什么原因没有监听到，所以就用了最原始的方法直接获取DOM元素，然后给元素的src赋值，这样竟然就实现了
    document.querySelector('.share-img').setAttribute("src",url)
    })
```
踩坑点都已经标注在注释里，在此就不一一解释了，仅作记录。