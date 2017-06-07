---
title: Hexo 主题同步
tags:
  - Hexo
  - Next 主题
categories: Hexo
typora-copy-images-to: Hexo-theme-sync
typora-root-url: Hexo-theme-sync
date: 2017-06-06 22:15:58
---

主题同步的时候肯定会产生冲突，不要害怕冲突，有冲突是好事，还能增强对 Git 的使用，只能这样安慰自己了。

关于 主题 同步，考虑了好几天， 还是采用这个方案吧。



> 参考 ： [同步hexo主题](http://jxy.me/2015/03/21/hexo-theme-sync/)

主题作者一直在增加新功能，我偶尔会修改下主题的样式，如何应用到我自己的站点里？
其实就是git的submodule的用法。

```
# 首先clone我自己的 repo 到 Hexo 的根目录 ， 注意最后的 ` themes/next`
git clone <hexo-theme-next@bitbucket.org>  themes/next
# cd themes/next 并 添加一个github的remote
cd themes/next
git remote add github git@github.com:iissnan/hexo-theme-next.git  # 注意这是不是 origin , origin 其实是默认的别名
# 将作者的一些更新合并到我自己的分支
git pull github master   # 注意这是不是 origin
git checkout foolbear
git merge master
# 肯定会有些冲突，解决冲突后commit
git commit -m "xxx"
# 修改后的foolbear分支push到我自己的repo
git push origin foolbear
```

以下操作在我自己笔记本上

```
cd themes/next
git pull
git checkout foolbear
```

概念上是很简单的，只是记录下。