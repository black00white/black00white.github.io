---
title: el-tabs异步按需加载el-tab-pane的内容
date: 2019-09-23 14:19:43
tags:
---
目前的项目是vue+element ui搭建的，其中需要用到el-tabs来做标签页来回切换，以显示不同的内容，后台调的都是同一个接口，只是传的参数不同，但是用过el-tabs的童鞋都知道，这个控件是直接将全部的内容都加载出来的，既浪费服务器资源，又不能满足实际业务场景需求，于是在写了过程中做了些优化：

首先是html代码

通过给每个组件添加条件判断来控制组件的显示，默认显示第一个，其余的全部设置为false
```
<el-tabs v-model="activeName" @tab-click="handleClick">
    <el-tab-pane label="全部" name="first">
        <orderTable v-if="load.first"/>
    </el-tab-pane>
    <el-tab-pane label="待接单" name="second">
        <orderTable v-if="load.second"/>
    </el-tab-pane>
    <el-tab-pane label="待上门" name="third">
        <orderTable v-if="load.third"/>
    </el-tab-pane>
  </el-tab-pane>
</el-tabs>
```

js代码段：

点击时通过点击事件控制让点击的tab的判断条件变为true，从而让改tab显示
```
export default {
    components: {orderTable},
    data() {
        return {
            activeName:'first',
            load:{
                first:true,
                second:false,
                third:false,
            },
            status:'',
        }
    },
    methods: {
        handleClick(tab,event){
            if (this.load[tab.name] === false){
                this.load[tab.name] = true
            }
        },
    }
}
```
再用watch监测一下activeName,通过activeName值的变化来改变参数中status的值，传给后台从而获得不同的数据展示：
```
watch: {
    activeName(newVal, oldVal){
        switch (newVal) {
            case 'first':this.status = null
            break;
            case 'second':this.status = '1'
            break;
            case 'third':this.status = '2'
            break;
            default:this.status = null
            break;
        }
    }
}
```
至此大功告成，如有不对之处欢迎之处，联系邮箱：[libeibei0804@outlook.com](libeibei0804@outlook.com)