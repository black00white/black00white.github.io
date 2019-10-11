---
title: git常用命令整理
date: 2019-09-28 11:44:08
tags:
---
整理下常用到的命令，方便自己用时查询

<font color=#409EFF face="微软雅黑">**基础应用**</font>

添加修改(*是指添加所有修改，也可以自定义添加)
```
git add *
```
提交
```
git commit "添加信息"
```
提交到远程分支（默认分支是master,若想提交到其他分支，改为其他分支名即可）
```
git push origin 分支名
```
切换到其它分支
```
git checkout 分支名
```
查看提交历史
```
git log
```
<font color=#409EFF face="微软雅黑">**关于打标签**</font>

打轻量级标签
```
git tag v1.0
```
默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，就需要先找到需要打标签的commit，然后再打标签
```
git log//查看历史提交，找到需要打标签的commit
git tag v1.0 <commit id>
```

查看标签
```
git tag
```

推送某个标签到远程仓库
```
git push origin <tagname>(v1.0)
```

推送全部未推送过的标签到远程仓库
```
git push origin --tags
```
先想到这么多，以后用到的再慢慢补充。