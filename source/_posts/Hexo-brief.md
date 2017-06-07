---
title: 记录搭建 Hexo 的一些步骤
tags:
  - Hexo
categories: Hexo
typora-copy-images-to: Hexo-brief
typora-root-url: Hexo-brief
date: 2017-06-06 22:15:58
---



总想有个自己的地方记录一些东西， 兜兜转转，还是选择了[GitHub Pages](https://pages.github.com/)。
试过[Jekyll](http://jekyllrb.com/)，可惜ruby跟我相性实在不合，折腾了半天也没搞明白。
于是选择了[Hexo](http://hexo.io)。使用很简单，js也比ruby亲切些，但搭建过程中还是碰到一些问题，这里记录下。



# 安装 Node.js 

暂时略， 可参考 [记录下搭建hexo的一些问题 by <foolbear的冥想盆>](http://jxy.me/2015/02/23/foolbear-hexo-init/)

[自制node.js + npm绿色版](http://ibruce.info/2013/12/05/green-node-and-npm/)



# 安装 Git

略， 作为开发， 你电脑上都没有 Git ？ 教程网上一大把。

建议： 安装的时候， 不要选择 **checkout Window ，commit Linux** ， 这样自动转换 EOL 的 默认选项。



# 安装 Hexo 【重头戏开始了】

写博客时 2017年6月6日23:46:34 的 版本信息 ，Hexo 不同的版本变化还挺大的。

```
PS ~\git\HexoBlogSource> hexo version
hexo: 3.3.7
hexo-cli: 1.0.3
os: Windows_NT 10.0.14393 win32 x64
http_parser: 2.7.0
node: 6.9.2
v8: 5.1.281.88
uv: 1.9.1
zlib: 1.2.8
ares: 1.10.1-DEV
icu: 57.1
modules: 48
openssl: 1.0.2j
```

[Hexo 官网](https://hexo.io/) 给出了 最简单的 安装步骤 (稍加整理)

```
# npm安装时其实可以指定版本号
npm install hexo-cli -g
hexo init blog
cd blog
# 安装一些必要的依赖 ， 执行前看下 <【知识点】在中国，npm 使用优化 >
npm install
hexo s[erver] -p 80    # [] 方括号中的内容可省略 
# 可选，使用rss时才需要
npm install hexo-generator-feed --save
# 可选，生成sitemap
npm install hexo-generator-sitemap --save
```

## 【知识点】在中国，npm 使用优化

- 网络不好、被墙的话，npm会很慢，试试加参数`--registry=http://r.cnpmjs.org`，国内的镜像。
- 出现各种奇怪的错误的话，试试`npm cache clear`。

#### 将 Npm 的源替换成淘宝的源
> 参考 ： [将 Npm 的源替换成淘宝的源  by sg.ff @2016年02月17日发布](https://segmentfault.com/a/1190000004444283)

在墙内久了，难免会碰到撞墙的时候，所幸国内也有众多 NPM 镜像可供选择，在大多数情况下我们可以使用国内的源（比如 [淘宝 NPM 镜像 http://npm.taobao.org](http://npm.taobao.org/)）去替换官方的源以加快下载包的速度。

不过呢，我们在发布自己的包的时候却需要将源修改回官方的 [https://registry.npmjs.org](https://registry.npmjs.org) 源。

```
## 修改源地址为淘宝 NPM 镜像

npm config set registry http://registry.npm.taobao.org/



## 修改源地址为官方源

npm config set registry https://registry.npmjs.org/
```



# 安装 Next 主题

> hexo的主题很多，但感觉好看的不多。。。
> 最终选择了[NexT](https://github.com/iissnan/hexo-theme-next)，很喜欢这种简洁的风格，作者更新也很积极。 **你会爱上 Next 的** 。



Hexo 主题的同步目前是个问题， 详见 另一篇文章 《Hexo 主题同步》



## Next 主题优化

内容太多， 详见 另一篇文章 《Hexo 主题优化》



# 未解之谜

> 可能的原因：
> > 参考自： [Hexo部署常见问题解决方案](http://wp.huangshiyang.com/hexo%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
>
> 将自己的`Blog`文件夹使用`Git`来管理，需要注意一下几点。
>
> 1. 如果主题是通过git管理的，需要将主题文件夹下的`.git`文件夹删除，才能同步Blog文件夹（.git文件夹是隐藏的，需要显示隐藏文件才能删除，Linux下需要`rm -rf`命令才能删除，Mac没用过，不清楚）。
> 2. 按照Blog目录下自带的`.gitignore`文件，`node_modules`文件夹是不会同步的，所以同步之后需要自己再次进行`npm install`，但是注意，不要进行`hexo init`了，否则`_config.yml`全都白弄了。

可以看到 `hexo init a` 时， 会自动把 `landscape` 下载好， 日志中也打出了 `Submodule 'themes/landscape' (...) registered for path 'themes/landscape'` 这样的信息， 但是：

奇怪的就是 ， landscape 在 Hexo 根目录 用 `git gui` 命令 是可以看到 文件的， 但是 next 主题只显示 subproject 一个文件， 难道是因为 `hexo init a` 用了类似 `git clone --recursive` 的语法，然后就不作为一个 subproject 了？

然后我把 landscape 目录删了， 按 Next 方式 clone 了一遍， landscape 目录也像 next 那样只显示 subproject 一个文件， 这样也好，减少了很多文件。

```
PS ~\git> hexo init a
INFO  Cloning hexo-starter to ~\git\a
Cloning into '~\git\a'...
remote: Counting objects: 53, done.
remote: Total 53 (delta 0), reused 0 (delta 0), pack-reused 53
Unpacking objects: 100% (53/53), done.
Submodule 'themes/landscape' (https://github.com/hexojs/hexo-theme-landscape.git) registered for path 'themes/landscape'

Cloning into '~/git/a/themes/landscape'...
remote: Counting objects: 775, done.
remote: Total 775 (delta 0), reused 0 (delta 0), pack-reused 775
Receiving objects: 100% (775/775), 2.53 MiB | 1.18 MiB/s, done.
Resolving deltas: 100% (397/397), done.

INFO  Install dependencies
��Ϣ: ���ṩ��ģʽ�޷��ҵ��ļ�
npm WARN deprecated swig@1.4.2: This package is no longer maintained
npm WARN prefer global marked@0.3.6 should be installed with -g

> fsevents@1.1.1 install ~\git\a\node_modules\fsevents
> node install


> dtrace-provider@0.8.2 install ~\git\a\node_modules\dtrace-provider
> node scripts/install.js


> hexo-util@0.6.0 postinstall ~\git\a\node_modules\hexo-util
> npm run build:highlight

npm WARN invalid config loglevel="notice"

> hexo-util@0.6.0 build:highlight ~\git\a\node_modules\hexo-util
> node scripts/build_highlight_alias.js > highlight_alias.json

npm notice created a lockfile as package-lock.json. You should commit this file.
added 438 packages in 200.46s
INFO  Start blogging with Hexo!
```















---

参考 ： 

- 【重点参考】[记录下搭建hexo的一些问题 ](http://jxy.me/2015/02/23/foolbear-hexo-init/)
- [最简便的方法搭建Hexo+Github博客,基于Next主题](http://blog.csdn.net/tx874828503/article/details/51577815)    
  - Hexo部署常见问题解决方案 – [传送门](http://wp.huangshiyang.com/hexo%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)

----

