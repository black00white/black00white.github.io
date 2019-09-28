---
title: el-radio选中时再次点击取消选中
date: 2019-09-28 11:26:30
tags:
---
用el-radio过程中遇到的需求，需要再次点击时取消选中，本来以为要用多选框然后来限制可选个数了，猛然间发现还有这个可以来解决

html部分
```
<el-radio-group v-model="radio1">
    <el-radio @click.native.prevent="clickitem(2)" :label="1">选项一</el-radio>
    <el-radio @click.native.prevent="clickitem(1)" :label="2">选项二</el-radio>
</el-radio-group>
```
```
export default {
    data () {
        return {
            radio1:1
        }
    },
    methods:{
        clickitem(e){
            e === this.radio1 ? this.radio1 = '' : this.radio1 = e
        }
    }
}
```

这样功能就实现了，然后发现点击取消后，选项前的圆圈那里还有阴影，虽然失去焦点之后就没有了，但是看这很不爽，还是把他干掉了
```
.el-radio:focus:not(.is-focus):not(:active):not(.is-disabled) .el-radio__inner{
        box-shadow: none;
}
```

把这个box-shadow禁掉就ok啦。完美解决。