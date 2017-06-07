---
title: Hexo 的 next 主题优化/个性化
tags:
  - Hexo
  - Next 主题
categories: Hexo
typora-copy-images-to: Hexo-theme-next-optimize
typora-root-url: Hexo-theme-next-optimize
date: 2017-06-06 22:15:58
top: 1000
password: optimize
---

参考自：

- [Hexo 官方 Doc](https://hexo.io/zh-cn/docs/index.html)
- [NexT 官方  Doc](http://theme-next.iissnan.com/getting-started.html)
- 【主要来源】[hexo的next主题个性化教程:打造炫酷网站](http://shenzekun.cn/hexo%E7%9A%84next%E4%B8%BB%E9%A2%98%E4%B8%AA%E6%80%A7%E5%8C%96%E9%85%8D%E7%BD%AE%E6%95%99%E7%A8%8B.html)
- [Hexo常见问题解决方案](http://wp.huangshiyang.com/hexo%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
- [hexo优化](http://blog.csdn.net/hubojing/article/details/54671131)

---

# 0. 基本配置

## 部署到多平台

> 参考 ： [将hexo-github博客同步托管至coding](http://blog.csdn.net/hubojing/article/details/51159454)
>
> > 由于百度蜘蛛把github屏蔽了，导致百度搜不到博客文章，let me feel a little unhappy，为了扩大网站收录率，我将hexo-github博客同步托管到国内coding上。

Hexo提供了[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)工具，可以帮助[部署Hexo到很多平台](https://hexo.io/zh-cn/docs/deployment.html)。

```
$ npm install hexo-deployer-git --save
```

修改_config.yml中的配置：

```
deploy:
  type: git
  repo:
    coding: git@git.coding.net:yafey/blog.git
    github: git@github.com:yafey/blog.git
  branch: master
```

然后执行命令：

```
hexo g  # 生成， hexo generate 的缩写
hexo d  # 部署， hexo deploy 的缩写
```



> 以下参考自 ： [Hexo博客搭建之使用Hexo+GitHub搭建个人博客](https://yanyinhong.github.io/2017/05/01/How-to-build-blog-with-hexo-and-github/)

在使用框架时，会常用到以下几个命令：

```
$ hexo generate  // 简写：hexo g，生成静态文件，会在当前目录下生成一个public文件夹
$ hexo server       // 简写：hexo s，启动本地服务，用于博客的预览
$ hexo deploy       // 简写：hexo d，部署到远程（如GitHub，可以在_config.yml中配置）
$ hexo new post-name // 简写：hexo n post-name， 新建文章
$ hexo new page page-name   //简写：hexo n page page-name，新建页面
```

还有常用的组合命令：

```
$ hexo d -g                 // 生成和部署
$ hexo s -g                 // 生成和预览
```

草稿命令：

```
$ hexo new draft <title>        // 新建草稿，存放在source/_drafts
$ hexo publish post <title>     // 发布草稿为文章，文章转移到source/_posts
$ hexo s -g --drafts            // 显示草稿
```



## 简单必要设置
> 注意：配置文件要符合英文标点符号使用规范: 冒号后必须空格，否则会编译错误

a. Hexo _config.yml 修改语言

```
language:
    - zh-Hans  # 中文
    - defaults
```

b. next _config.yml 修改 主题  

```
scheme: Mist  #整洁有序的单栏外观 ， 右侧边栏
```

c. 网站标题，作者
Hexo _config.yml ，修改这些值：
```
title: Y.a.F.e.Y Blog #博客标题
subtitle: #博客副标题
description:  #博客描述
author: YaFey #作者
timezone: Asia/Chongqing
```

d. 域名，文章链接

Hexo _config.yml ，修改这些值：
```
url: http://xxx #你的博客网址
root: /blog/	#博客跟目录，如果你的博客在网址的二级目录下，在这里填上
permalink: :year/:month/:day/:title/
permalink_defaults: #可以不填
```

e. 创建页面 & 修改菜单

[添加「标签」页面](http://theme-next.iissnan.com/theme-settings.html#tags-page)

[添加「分类」页面](http://theme-next.iissnan.com/theme-settings.html#categories-page)

标签 和 分类 禁用评论

```
---
title: categories  # 若 标签， title 为 tags
date: 2017-05-28 09:53:57
type: "categories"   # 若 标签， 引号内 为 tags
comments: false #禁用评论
---
```






# 1. next 主题优化

> 看到有些next主题的网站很炫酷，那么是怎么配置的呢？接下来我会讲一讲如何实现一些炫酷的效果

主要有以下29种：

- 在右上角或者左上角实现fork me on github
- 添加RSS
- 添加动态背景
- 实现点击出现桃心效果
- 修改文章内链接文本样式
- 修改文章底部的那个带#号的标签
- 在每篇文章末尾统一添加“本文结束”标记
- 修改作者头像并旋转
- 博文压缩
- 修改``代码块自定义样式
- 侧边栏社交小图标设置
- 主页文章添加阴影效果
- 在网站底部加上访问量
- 添加热度
- 网站底部字数统计
- 添加 README.md 文件
- 设置网站的图标Favicon
- 实现统计功能
- 添加顶部加载条
- 在文章底部增加版权信息
- 添加网易云跟帖
- 隐藏网页底部powered By Hexo / 强力驱动
- 修改网页底部的桃心
- 文章加密访问
- 添加jiathis分享
- 博文置顶
- 修改字体大小
- 修改打赏字体不闪动
- 侧边栏推荐阅读

------



## 0. 无痛使用图片

参考另一篇文章 [在 hexo 中无痛使用本地图片](/no-pains-with-hexo-local-image/)



## 1. 在右上角或者左上角实现fork me on github

**实现效果图**

![1496848132026](1496848132026.png)

 **具体实现方法**

点击[这里](https://github.com/blog/273-github-ribbons)挑选自己喜欢的样式，并复制代码。 例如，我是复制如下代码：
![1496848208963](1496848208963.png)
然后粘贴刚才复制的代码到`themes/next/layout/_layout.swig`文件中(放在`<div class="headband"></div>`的下面)，并把`href`改为你的github地址
![1496848499823](1496848499823.png)

------

## 2.添加RSS

**实现效果图**

![1496848539174](1496848539174.png)

**具体实现方法**

切换到你的blog（我是取名blog，具体的看你们的取名是什么）的路径，例如我是在`/Users/chenzekun/Code/Hexo/blog`这个路径上，也就是在你的根目录下
![1496848581473](1496848581473.png)

然后安装 Hexo 插件：(这个插件会放在`node_modules`这个文件夹里)

```
$ npm install --save hexo-generator-feed
```

接下来打开画红线的文件，如下图：

![1496848702783](1496848702783.png)

在里面的末尾添加：(**请注意**在冒号后面要加一个空格，不然会发生错误！)

```
# Extensions
## Plugins: http://hexo.io/plugins/
plugins: hexo-generate-feed
```

然后打开next主题文件夹里面的`_config.yml`,在里面配置为如下样子：(就是在`rss:`的后面加上`/atom.xml`,**注意**在冒号后面要加一个空格)

```
# Set rss to false to disable feed link.
# Leave rss as empty to use site's feed link.
# Set rss to specific value if you have burned your feed already.
rss: /atom.xml
```

配置完之后运行：

```
$ hexo g
```

重新生成一次，你会在`./public` 文件夹中看到 `atom.xml` 文件。然后启动服务器查看是否有效，之后再部署到 Github 中。

------

## 3. 添加动态背景

**实现效果图**

![img](2f49ca0c.gif)

**具体实现方法**

~~这个我之前有一篇文章有讲过了，详情点击[我的博客](http://shenzekun.cn/hexo%E5%A6%82%E4%BD%95%E6%B7%BB%E5%8A%A0%E5%8A%A8%E6%80%81%E8%83%8C%E6%99%AF.html)~~

### 修改`_layout.swig`

打开 `next/layout/_layout.swig`
在 `< /body>`之前添加代码(注意不要放在< /head>的后面)

```
{% if theme.canvas_nest %}
<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

------

### 修改配置文件

打开 `/next/_config.yml`,在里面添加如下代码：(可以放在最后面)

```
# --------------------------------------------------------------
# background settings
# --------------------------------------------------------------
# add canvas-nest effect
# see detail from https://github.com/hustcc/canvas-nest.js
canvas_nest: true
```

到此就结束了，运行 `hexo clean`，然后运行 `hexo g`,然后运行 `hexo s`，最后打开浏览器在浏览器的地址栏输入 `localhost:4000` 就能看到效果了\（￣︶￣）/

------

### 如果你感觉默认的线条太多的话

#### 可以这么设置====>

在上一步修改 `_layout.swig`中，把刚才的这些代码：

```
{% if theme.canvas_nest %}
<script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

改为

```
{% if theme.canvas_nest %}
<script type="text/javascript"
color="0,0,255" opacity='0.7' zIndex="-2" count="99" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
{% endif %}
```

------

### 配置项说明

- `color` ：线条颜色, 默认: `'0,0,0'`；三个数字分别为(R,G,B)
- `opacity`: 线条透明度（0~1）, 默认: `0.5`
- `count`: 线条的总数量, 默认: `150`
- `zIndex:` 背景的z-index属性，css属性用于控制所在层的位置, 默认: `-1`

------

### 4. 实现点击出现桃心效果

**实现效果图**

![img](strip.gif)

 **具体实现方法**

在网址输入如下

```
http://7u2ss1.com1.z0.glb.clouddn.com/love.js
```

然后将里面的代码copy一下，新建`love.js`文件并且将代码复制进去，然后保存。将`love.js`文件放到路径`/themes/next/source/js/src`里面，然后打开`\themes\next\layout\_layout.swig`文件,在末尾（在前面引用会出现找不到的bug）添加以下代码：

```
  <!-- 页面点击小红心 -->
  {% include '_third-party/love.swig' %}
```

新建 `love.swig`

```
<script type="text/javascript" src="{{ url_for(theme.js) }}/src/love.js"></script>
```



------

# 5. 修改文章内链接文本样式

**实现效果图**

![img](5308475-8cc4fc18c399af7e.gif)

 **具体实现方法**

修改文件 `themes\next\source\css\_common\components\post\post.styl`，在末尾添加如下css样式，：

```
// 文章内链接文本样式
.post-body p a{
  color: #0593d3;
  border-bottom: none;
  border-bottom: 1px solid #0593d3;
  &:hover {
    color: #fc6423;
    border-bottom: none;
    border-bottom: 1px solid #fc6423;
  }
}
```

其中选择`.post-body` 是为了不影响标题，选择 `p` 是为了不影响首页“阅读全文”的显示样式,颜色可以自己定义。

------

### 6. 修改文章底部的那个带#号的标签

**实现效果图**

![1496849376378](1496849376378.png)

**具体实现方法**

修改模板`/themes/next/layout/_macro/post.swig`，搜索 `rel="tag">#`，将 # 换成`<i class="fa fa-tag"></i>`

------

### 7. 在每篇文章末尾统一添加“本文结束”标记

**实现效果图**

![1496849605921](1496849605921.png)

**具体实现方法**

在路径 `\themes\next\layout\_macro` 中新建 `passage-end-tag.swig` 文件,并添加以下内容：

```
<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">-------------本文结束<i class="fa fa-paw"></i>感谢您的阅读-------------</div>
    {% endif %}
</div>
```

接着打开`\themes\next\layout\_macro\post.swig`文件，在`post-body` 之后， `post-footer` 之前添加如下画红色部分代码（post-footer之前两个DIV）：

![1496849729676](1496849729676.png)

代码如下：

```
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```

然后打开主题配置文件（`_config.yml`),在末尾添加：

```
# 文章末尾添加“本文结束”标记
passage_end_tag:
  enabled: true
```

完成以上设置之后，在每篇文章之后都会添加如上效果图的样子。

------

# 8. 修改作者头像并旋转

**实现效果图**

![img](5308475-e035ce8ac8ea0b5b.gif)

**具体实现方法**

打开`\themes\next\source\css\_common\components\sidebar\sidebar-author.styl`，在里面添加如下代码：

```
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;

  /* 头像圆形 */
  border-radius: 80px;
  -webkit-border-radius: 80px;
  -moz-border-radius: 80px;
  box-shadow: inset 0 -1px 0 #333sf;

  /* 设置循环动画 [animation: (play)动画名称 (2s)动画播放时长单位秒或微秒 (ase-out)动画播放的速度曲线为以低速结束
    (1s)等待1秒然后开始动画 (1)动画播放次数(infinite为循环播放) ]*/


  /* 鼠标经过头像旋转360度 */
  -webkit-transition: -webkit-transform 1.0s ease-out;
  -moz-transition: -moz-transform 1.0s ease-out;
  transition: transform 1.0s ease-out;
}

img:hover {
  /* 鼠标经过停止头像旋转
  -webkit-animation-play-state:paused;
  animation-play-state:paused;*/

  /* 鼠标经过头像旋转360度 */
  -webkit-transform: rotateZ(360deg);
  -moz-transform: rotateZ(360deg);
  transform: rotateZ(360deg);
}

/* Z 轴旋转动画 */
@-webkit-keyframes play {
  0% {
    -webkit-transform: rotateZ(0deg);
  }
  100% {
    -webkit-transform: rotateZ(-360deg);
  }
}
@-moz-keyframes play {
  0% {
    -moz-transform: rotateZ(0deg);
  }
  100% {
    -moz-transform: rotateZ(-360deg);
  }
}
@keyframes play {
  0% {
    transform: rotateZ(0deg);
  }
  100% {
    transform: rotateZ(-360deg);
  }
}
```

------

### 9. 博文压缩

在站点的根目录下执行以下命令：

```
$ npm install gulp -g
$ npm install gulp-clean-css gulp-uglify gulp-htmlmin gulp-htmlclean gulp --save
```

在如下图所示，新建 `gulpfile.js` ，并填入以下内容：

![1496850025733](1496850025733.png)

```
var gulp = require('gulp');
var minifycss = require('gulp-clean-css');
var uglify = require('gulp-uglify');
var htmlmin = require('gulp-htmlmin');
var htmlclean = require('gulp-htmlclean');
// 压缩 public 目录 css
gulp.task('minify-css', function() {
    return gulp.src('./public/**/*.css')
        .pipe(minifycss())
        .pipe(gulp.dest('./public'));
});
// 压缩 public 目录 html
gulp.task('minify-html', function() {
  return gulp.src('./public/**/*.html')
    .pipe(htmlclean())
    .pipe(htmlmin({
         removeComments: true,
         minifyJS: true,
         minifyCSS: true,
         minifyURLs: true,
    }))
    .pipe(gulp.dest('./public'))
});
// 压缩 public/js 目录 js
gulp.task('minify-js', function() {
    return gulp.src('./public/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('./public'));
});
// 执行 gulp 命令时执行的任务
gulp.task('default', [
    'minify-html','minify-css','minify-js'
]);
```

生成博文时执行 `hexo g && gulp` 就会根据 `gulpfile.js` 中的配置，对 public 目录中的静态资源文件进行压缩。

------

### 10. 修改``代码块自定义样式

**实现效果图**

![1496850304813](1496850304813.png)

**具体实现方法**

打开`\themes\next\source\css\_custom\custom.styl`,向里面加入：(颜色可以自己定义)

```
// Custom styles.
code {
    color: #ff7600;
    background: #fbf7f8;
    margin: 2px;
}
// 大代码块的自定义样式
.highlight, pre {
    margin: 5px 0;
    padding: 5px;
    border-radius: 3px;
}
.highlight, code, pre {
    border: 1px solid #d6d6d6;
}
```

------

# 11. 侧边栏社交小图标设置

**实现效果图**

![1496850405483](1496850405483.png)

**具体实现方法**

打开主题配置文件（`_config.yml`），搜索`social_icons:`,在[图标库](http://fontawesome.io/icons/)找自己喜欢的小图标，并将名字复制在如下位置，保存即可

![1496850443262](1496850443262.png)

------

### 12. 主页文章添加阴影效果

**实现效果图**

![1496850472194](1496850472194.png)

**具体实现方法**

打开`\themes\next\source\css\_custom\custom.styl`,向里面加入：

```
// 主页文章添加阴影效果
 .post {
   margin-top: 60px;
   margin-bottom: 60px;
   padding: 25px;
   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
  }
```

------

### 13. 在网站底部加上访问量

**实现效果图**

![1496850538531](1496850538531.png)

**具体实现方法**
打开`\themes\next\layout\_partials\footer.swig`文件,在copyright前加上画红线这句话：
![1496850783220](1496850783220.png)

代码如下：

```
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```

然后再合适的位置添加显示统计的代码，如图：
![1496850810318](1496850810318.png)
代码如下：

```
<div class="powered-by">
<i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
  本站访客数:<span id="busuanzi_value_site_uv"></span>
</span>
</div>
```

在这里有两中不同计算方式的统计代码：

1. **pv**的方式，单个用户连续点击n篇文章，记录n次访问量

```
<span id="busuanzi_container_site_pv">
    本站总访问量<span id="busuanzi_value_site_pv"></span>次
</span>
```

1. **uv**的方式，单个用户连续点击n篇文章，只记录1次访客数

```
<span id="busuanzi_container_site_uv">
  本站总访问量<span id="busuanzi_value_site_uv"></span>次
</span>
```

添加之后再执行`hexo d -g`，然后再刷新页面就能看到效果

------

### 14. 添加热度

**实现效果图**

![1496850844202](1496850844202.png)

**具体实现方法**

**next**主题集成**leanCloud**，打开`/themes/next/layout/_macro/post.swig`,在画红线的区域添加`℃`：

![1496850882223](1496850882223.png)

然后打开，`/themes/next/languages/zh-Hans.yml`,将画红框的改为热度就可以了
![1496850908305](1496850908305.png)

------

### 15. 网站底部字数统计

**实现效果图**

![1496851126712](1496851126712.png)

**具体方法实现**
切换到根目录下，然后运行如下代码

```
$ npm install hexo-wordcount --save
```

然后在`/themes/next/layout/_partials/footer.swig`文件尾部加上：

```
<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
</div>
```

------

# 16. 添加 README.md 文件

每个项目下一般都有一个 `README.md` 文件，但是使用 hexo 部署到仓库后，项目下是没有 `README.md` 文件的。

在 Hexo 目录下的 `source` 根目录下添加一个 `README.md` 文件，修改站点配置文件 _`config.yml`，将 `skip_render` 参数的值设置为

```
skip_render: README.md
```

保存退出即可。再次使用 `hexo d` 命令部署博客的时候就不会在渲染 README.md 这个文件了。

------

# 17. 设置网站的图标Favicon

**实现效果图**

![1496851503834](1496851503834.png)

**具体方法实现**

在[EasyIcon](http://www.easyicon.net/)中找一张（32*32）的`ico`图标,或者去别的网站下载或者制作，并将图标名称改为`favicon.ico`，然后把图标放在`/themes/next/source/images`里，并且修改主题配置文件：

```
# Put your favicon.ico into `hexo-site/source/` directory.
favicon: /favicon.ico
```

------

### 18. 实现统计功能

**实现效果图**
![1496851532217](1496851532217.png)

**具体实现方法**

在根目录下安装 `hexo-wordcount`,运行：

```
$ npm install hexo-wordcount --save
```

然后在主题的配置文件中，配置如下：

```
# Post wordcount display settings
# Dependencies: https://github.com/willin/hexo-wordcount
post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
```

------

### 19. 添加顶部加载条

**实现效果图**

![img](5308475-2f5051d9f0352b90.gif)

**具体实现方法**

打开`/themes/next/layout/_partials/head.swig`文件，添加红框上的代码
![1496851626700](1496851626700.png)
代码如下：

```
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
```

但是，默认的是粉色的，要改变颜色可以在`/themes/next/layout/_partials/head.swig`文件中添加如下代码（接在刚才link的后面）

```
<style>
    .pace .pace-progress {
        background: #1E92FB; /*进度条颜色*/
        height: 3px;
    }
    .pace .pace-progress-inner {
         box-shadow: 0 0 10px #1E92FB, 0 0 5px     #1E92FB; /*阴影颜色*/
    }
    .pace .pace-activity {
        border-top-color: #1E92FB;    /*上边框颜色*/
        border-left-color: #1E92FB;    /*左边框颜色*/
    }
</style>
```

> 目前，博主的增加顶部加载条的pull request 已被Merge😀===>[详情](https://github.com/iissnan/hexo-theme-next/pull/1689)
> 现在升级最新版的next主题，升级后只需修改主题配置文件(_config.yml)将`pace: false`改为`pace: true`就行了，你还可以换不同样式的加载条，如下图：
> ![1496851663136](1496851663136.png)

------

### 20. 在文章底部增加版权信息

**实现效果图**

![1496851692301](1496851692301.png)

在目录 `next/layout/_macro/下`添加 `my-copyright.swig`：

```
{% if page.copyright %}
<div class="my_post_copyright">
  <script src="//cdn.bootcss.com/clipboard.js/1.5.10/clipboard.min.js"></script>

  <!-- JS库 sweetalert 可修改路径 -->
  <script type="text/javascript" src="http://jslibs.wuxubj.cn/sweetalert_mini/jquery-1.7.1.min.js"></script>
  <script src="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.min.js"></script>
  <link rel="stylesheet" type="text/css" href="http://jslibs.wuxubj.cn/sweetalert_mini/sweetalert.mini.css">
  <p><span>本文标题:</span><a href="{{ url_for(page.path) }}">{{ page.title }}</a></p>
  <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
  <p><span>发布时间:</span>{{ page.date.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>最后更新:</span>{{ page.updated.format("YYYY年MM月DD日 - HH:MM") }}</p>
  <p><span>原始链接:</span><a href="{{ url_for(page.path) }}" title="{{ page.title }}">{{ page.permalink }}</a>
    <span class="copy-path"  title="点击复制文章链接"><i class="fa fa-clipboard" data-clipboard-text="{{ page.permalink }}"  aria-label="复制成功！"></i></span>
  </p>
  <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="https://creativecommons.org/licenses/by-nc-nd/4.0/" target="_blank" title="Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)">署名-非商业性使用-禁止演绎 4.0 国际</a> 转载请保留原文链接及作者。</p>  
</div>
<script>
    var clipboard = new Clipboard('.fa-clipboard');
	clipboard.on('success', $(function(){
	  $(".fa-clipboard").click(function(){
		swal({   
		  title: "",   
		  text: '复制成功',   
		  html: false,
		  timer: 500,   
		  showConfirmButton: false
	    });
	  });
    }));  
</script>
{% endif %}
```

在目录`next/source/css/_common/components/post/`下添加`my-post-copyright.styl`：

```
.my_post_copyright {
  width: 85%;
  max-width: 45em;
  margin: 2.8em auto 0;
  padding: 0.5em 1.0em;
  border: 1px solid #d3d3d3;
  font-size: 0.93rem;
  line-height: 1.6em;
  word-break: break-all;
  background: rgba(255,255,255,0.4);
}
.my_post_copyright p{margin:0;}
.my_post_copyright span {
  display: inline-block;
  width: 5.2em;
  color: #b5b5b5;
  font-weight: bold;
}
.my_post_copyright .raw {
  margin-left: 1em;
  width: 5em;
}
.my_post_copyright a {
  color: #808080;
  border-bottom:0;
}
.my_post_copyright a:hover {
  color: #a3d2a3;
  text-decoration: underline;
}
.my_post_copyright:hover .fa-clipboard {
  color: #000;
}
.my_post_copyright .post-url:hover {
  font-weight: normal;
}
.my_post_copyright .copy-path {
  margin-left: 1em;
  width: 1em;
  +mobile(){display:none;}
}
.my_post_copyright .copy-path:hover {
  color: #808080;
  cursor: pointer;
}
```

修改`next/layout/_macro/post.swig`，在代码

```
<div>
      {% if not is_index %}
        {% include 'wechat-subscriber.swig' %}
      {% endif %}
</div>
```

之前添加增加如下代码：

```
<div>
      {% if not is_index %}
        {% include 'my-copyright.swig' %}
      {% endif %}
</div>
```

如下：

![1496851964028](1496851964028.png)

修改`next/source/css/_common/components/post/post.styl`文件，在最后一行增加代码：

```
@import "my-post-copyright"
```

保存重新生成即可。
如果要在该博文下面增加版权信息的显示，需要在 Markdown 中增加copyright: true的设置，类似：

```
---
title: 前端小项目：使用canvas绘画哆啦A梦
date: 2017-05-22 22:53:53
tags: canvas
categories: 前端
copyright: true
---
```

> **小技巧**：如果你觉得每次都要输入`copyright: true`很麻烦的话,那么在`/scaffolds/post.md`文件中添加：
> ![1496852061457](1496852061457.png)
> 这样每次`hexo new "你的内容"`之后，生成的md文件会自动把`copyright:`加到里面去
> (**注意**：如果解析出来之后，你的原始链接有问题：如：`http://yoursite.com/前端小项目：使用canvas绘画哆啦A梦.html`,那么在根目录下`_config.yml`中写成类似这样：）
> ![1496852107939](1496852107939.png)就行了。

------

# 21. 添加网易云跟帖

**实现效果图**

[![img](http://upload-images.jianshu.io/upload_images/5308475-86b212c1221292ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/5308475-86b212c1221292ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**具体方法实现**
有两种实现方法：
①更新next主题，因为最新版本的主题已经支持这种评论。直接在主题配置文件`_config.yml` 文件中添加如下配置:

```

```

```
gentie_productKey: #your-gentie-product-key
```

②如果你不想更新的话，那么按下面步骤进行：
首先，还是在主题配置文件`_config.yml` 文件中添加如下配置:

```

```

```
gentie_productKey: #your-gentie-product-key
```

你的`productKey`就是下面画红线部分
[![img](http://upload-images.jianshu.io/upload_images/5308475-2d1eeef55db50cfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/5308475-2d1eeef55db50cfc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后在在`layout/_scripts/third-party/comments/` 目录中添加 `gentie.swig`，文件内容如下：

```

```

```
{% if not (theme.duoshuo and theme.duoshuo.shortname) and not theme.duoshuo_shortname and not theme.disqus_shortname and not theme.hypercomments_id %}
  {% if theme.gentie_productKey %}
    {% set gentie_productKey = theme.gentie_productKey %}
    <script>
      var cloudTieConfig = {
        url: document.location.href,
        sourceId: "",
        productKey: "{{gentie_productKey}}",
        target: "cloud-tie-wrapper"
      };
    </script>
    <script src="https://img1.ws.126.net/f2e/tie/yun/sdk/loader.js"></script>
  {% endif %}
{% endif %}
```

然后在`layout/_scripts/third-party/comments.swig`文件中追加：

```

```

```
{% include './comments/gentie.swig' %}
```

最后，在 `layout/_partials/comments.swig` 文件中条件最后追加网易云跟帖插件引用的判断逻辑：

```

```

```
{% elseif theme.gentie_productKey %}
      <div id="cloud-tie-wrapper" class="cloud-tie-wrapper">
      </div>
```

具体位置如下：
[![img](http://upload-images.jianshu.io/upload_images/5308475-c9199439d2396989.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/5308475-c9199439d2396989.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可能你`hexo s`时可能看不到，直接`hexo d`就可以看到了

------

# 22. 隐藏网页底部powered By Hexo / 强力驱动

打开`themes/next/layout/_partials/footer.swig`,使用””隐藏之间的代码即可，或者直接删除。位置如图：

![1496852254178](1496852254178.png)

------

# 23. 修改网页底部的桃心

还是打开`themes/next/layout/_partials/footer.swig`，找到：
![1496852284885](1496852284885.png)，然后还是在[图标库](http://fontawesome.io/icons/)中找到你自己喜欢的图标，然后修改画红线的部分就可以了。

------

### 24. 文章加密访问

**实现效果图**

![img](5308475-0c7e5e61b78dc937.gif)

**具体实现方法**

打开`themes->next->layout->_partials->head.swig`文件,在以下位置插入这样一段代码：

![1496852363252](1496852363252.png)

代码如下：

```
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误！');
                history.back();
            }
        }
    })();
</script>
```

然后在文章上写成类似这样：

![1496852496398](1496852496398.png)

------

# 25. 添加jiathis分享

在**主题配置文件**中，jiathis为true，就行了，如下图

[![img](http://upload-images.jianshu.io/upload_images/5308475-f381bc9e7c73b1e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/5308475-f381bc9e7c73b1e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

默认是这样子的：
[![img](http://upload-images.jianshu.io/upload_images/5308475-a88228b9ea43ab2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/5308475-a88228b9ea43ab2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果你想自定义话，打开`themes/next/layout/_partials/share/jiathis.swig`修改画红线部分就可以了
[![img](http://upload-images.jianshu.io/upload_images/5308475-68c51498744e1636.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/5308475-68c51498744e1636.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

------

### 26. 博文置顶

修改 `hero-generator-index` 插件，把文件：`node_modules/hexo-generator-index/lib/generator.js` 内的代码替换为：

```
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```

在文章中添加 `top` 值，数值越大文章越靠前，如

```
---
title: 解决Charles乱码问题
date: 2017-05-22 22:45:48
tags: 技巧
categories: 技巧
copyright: true
top: 100
---
```

------

### 27. 修改字体大小

打开`\themes\next\source\css\ _variables\base.styl`文件，将`$font-size-base`改成`16px`，如下所示：

```
$font-size-base            =16px
```

------

### 28. 修改打赏字体不闪动

修改文件`next/source/css/_common/components/post/post-reward.styl`，然后注释其中的函数`wechat:hover`和`alipay:hover`，如下：

```
/* 注释文字闪动函数
 #wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
 #alipay:hover p{
   animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
*/
```

------

### 29. 侧边栏推荐阅读

今天有位网友问推荐阅读是怎么弄，其实挺简单的，打开主题配置文件修改成这样就行了(links里面写你想要的链接):

```
# Blogrolls
links_title: 推荐阅读
#links_layout: block
links_layout: inline
links:
  优设: http://www.uisdc.com/
  张鑫旭: http://www.zhangxinxu.com/
  Web前端导航: http://www.alloyteam.com/nav/
  前端书籍资料: http://www.36zhen.com/t?id=3448
  百度前端技术学院: http://ife.baidu.com/
  google前端开发基础: http://wf.uisdc.com/cn/
```

------

# 致谢

> 感谢大神们的文章，真的学到了许多，有些忘了记录下来，在这里由衷的感谢。虽然比较折腾，但是确实满满的成就感，Road endless its long and far, I will seek up and down！

欢迎访问[我的博客](http://shenzekun.cn/)

------

#### 参考的文章：

1. [http://blog.csdn.net/MasterAnt_D/article/details/56839222](http://blog.csdn.net/MasterAnt_D/article/details/56839222)
2. [http://zidingyi4qh.com/2017/04/27/NexT%E5%BA%95%E9%83%A8logo%E6%B7%BB%E5%8A%A0%E8%AE%BF%E9%97%AE%E9%87%8F/](http://zidingyi4qh.com/2017/04/27/NexT%E5%BA%95%E9%83%A8logo%E6%B7%BB%E5%8A%A0%E8%AE%BF%E9%97%AE%E9%87%8F/)
3. [https://fuyis.me/2017/01/25/Hexo-theme-next-and-optimized-configuration/](https://fuyis.me/2017/01/25/Hexo-theme-next-and-optimized-configuration/)
4. [http://www.vitah.net/posts/20f300cc/](http://www.vitah.net/posts/20f300cc/)
5. [http://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/](http://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/)
