---
title: 在 hexo 中无痛使用本地图片
tags:
  - Hexo
categories: hexo
typora-copy-images-to: no-pains-with-hexo-local-image
typora-root-url: no-pains-with-hexo-local-image
date: 2017-06-01 23:13:03
---



>原文：http://www.tuicool.com/articles/umEBVfI
>~~http://codefalling.com/2015/12/19/no-pains-with-hexo-local-image/~~ 已失效

#       1 起因    

在 hexo 中使用本地图片是件非常让人纠结的事情，在 markdown 里的图片地址似乎永远无法和最后生成的网页保持一致。

这些问题使得我一度不愿意使用本地图片而选择用图床，但被移动运营商无耻的横条广告逼得打算上 https，图床只支持 http 就成了问题。

hexo 下插入图片现在大概有几个方案



##         1.1 放在根目录      

早期大部分的方案是把图片放在          `source/img`下，然后在 markdown 里写          `![img](/source/img/img.png)`。显然这样在本地的编辑器里完全不能正确识别图片的位置。        

##         1.2 asset-image      

在 hexo 2.x 时出现的插件，后来被吸纳进          `hexo 3 core`，用法的介绍见          [资源文件夹 | Hexo](https://hexo.io/zh-cn/docs/asset-folders.html)。比较尴尬的是，这种方法直接放弃了 markdown 原来的语法，使用类似          ``的语法，。markdown 本来有插入图片的语法不好好支持，专门用一个新的语法来插入本地图片，让我这种强迫症不太能接受。        

#       2 解决方案    

[CodeFalling/hexo-asset-image](https://github.com/CodeFalling/hexo-asset-image)      

##         2.1使用      

首先确认          `_config.yml`中有          `post_asset_folder:true`。        

在 hexo 目录，执行

```
npm install https://github.com/CodeFalling/hexo-asset-image --save

# 在 pull request merged 之前使用下面的
npm install https://github.com/yafey/hexo-asset-image --save
```

假设在

```
MacGesture2-Publish
├── apppicker.jpg
├── logo.jpg
└── rules.jpg
MacGesture2-Publish.md
```

这样的目录结构（目录名和文章名一致），只要使用          `![logo](MacGesture2-Publish/logo.jpg)`就可以插入图片。        

生成的结构为

```
public/2015/10/18/MacGesture2-Publish
├── apppicker.jpg
├── index.html
├── logo.jpg
└── rules.jpg
```

同时，生成的 html 是

```
<img src="/HexoBlog/2015/10/18/MacGesture2-Publish/logo.jpg" alt="logo">
```

而不是愚蠢的

```
<img src="MacGesture2-Publish/logo.jpg" alt="logo">

```

值得一提的是，这个插件对于*<u>( yafey 注：我现在还不知道这货是干嘛用的 --> )</u>*  [CodeFalling/hexo-renderer-org](https://github.com/CodeFalling/hexo-renderer-org)  同样有效。          



### 2.1.1 兼容性问题？

在本地使用过程中发现，在 使用了 `hexo-asset-image` 之后， 如果更新了文件， 预览的时候 文件就无法正常显示， 一定要 重新 `hexo g` ， 然后 `hexo s -p 80` 才能预览， 这个估计值得 enhance ， 可以去 提个 issue 。



## 2.2 第一次 raise a pull request to an open source project

因为我的 Hexo 现在是放在 xxx.github.io/HexoBlog 上的， hexo-asset-image 生成的 link 原先是 不带 HexoBlog 的，竟然被我稀里糊涂的改掉了， 那就 raise pull request  给 作者吧， 这算是我第一次给 open source 提 pull request ， 以后也可以说是 给 open source 做过贡献了 :)  。

pull request 链接 ： https://github.com/CodeFalling/hexo-asset-image/pull/25/files

![附上截图](1496330644115.png)
