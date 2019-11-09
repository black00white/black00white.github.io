---
title: 从后台获取的数组，改变数组之后，vue视图不更新的问题
date: 2019-11-09 11:02:25
tags:
---
后台返回的数据是一个数据，然后我把这个数组赋值给了data里边的一个变量，上传图片成功之后改变这个变量中的图片地址，结果在图片上传成功的函数中，打印出来的数据是已经变了的，但是vue的视图就是没变，可把我急坏了。线上代码看最初的写法：
```
getDataList(){//从后台获取原始数据
    this.adList = info//把获取到的info数据赋值给data里已经存在的变量adList
},
cardUpload1(val){
    this.adList[this.uploadTag].image_url = val.info.url//图片上传成功之后，替换原有的图片地址，结果在这里打印出来的adlist是已经变化的，但是html的视图就是不发生变化
},
```

然后查了一大堆资料才发现，vue对于数组变化的监听，有两种情况是监听不到的：

① 利用索引直接设置一个项时，vm.items[indexOfItem] = newValue

② 修改数组的长度时，例如： vm.items.length = newLength

而我最初的写法刚好就是属于第一种的改变数组的方式，这个时候想要改变数组，同时让视图更新，就需要用到数组的splice方法，该方法可以更改数组原来的值。所以修改后的写法为：
```
cardUpload1(val){
    var item = this.adList[this.uploadTag]
    item.image_url = val.info.url
    this.adList.splice(this.uploadTag, item)
},
```

<!-- 鉴于此，刚好扒一扒js数组的常见方法及返回值（主要涉及改变数组长度的，对比是否会引起视图变化）


push()	向数组的末尾添加元素，并返回新的长度。所以用push()改变的数组，vue的视图中也不会发生变化。

concat()	连接两个或更多的数组，并返回结果。 -->
