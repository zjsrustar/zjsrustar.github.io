---
title: nvm or n
tags:
  - node.js
categories: node.js
date: 2017-06-01 20:58:09
typora-copy-images-to: node-version-control-nvm-or-n
typora-root-url: node-version-control-nvm-or-n
---



# 如何选择 nvm 还是 n ?

> [管理 node 版本，选择 nvm 还是 n？@「淘宝前端团队(FED)」](http://taobaofed.org/blog/2015/11/17/nvm-or-n/)

nvm 和 n 的差异还是比较大的，具体体现在：

- 安装简易度。nvm 安装起来显然是要麻烦不少；n 这种安装方式更符合 node 的惯性思维。见仁见智吧。
- 系统支持。注意， ~~**nvm 不支持 Windows**~~   **n 不支持 windows**。
- 对全局模块的管理。n 对全局模块毫无作为，因此有可能在切换了 node 版本后发生全局模块执行出错的问题；nvm 的全局模块存在于各自版本的沙箱中，切换版本后需要重新安装，不同版本间也不存在任何冲突。
- 关于 node 路径。n 是万年不变的 `/usr/local/bin`；nvm 需要手动指定路径。

所以，如何选择？真心见仁见智了，不过这里可以给出大体的建议：

1. 如果你使用 Windows，那没得选了，使用 n，或者换一台 Mac。
2. 如果你会频繁切换 node 版本（比如本地经常测试最新版的特性，同时又要兼顾代码在生产环境的兼容性），那么从全局模块兼容性的角度考虑，只能使用 nvm。
3. 如果你是一个轻量级的用户，不需要担心兼容性的问题，更关心 node 安装和使用上的体验，那么选择 n。

你如果要问，楼主最终选用了谁？我会说，我选择了更流行的那一个。

![1496322408836](1496322408836.png)



# 升级node.js和npm [注]

> > 参考：https://segmentfault.com/a/1190000009025883
>
>   **<u>n 模块不支持windows系统</u>**，官方文档
> (Unfortunately n is not supported on Windows yet. If you're able to make it work, send in a pull request!)

一行命令搞定npm和node.js的升级，省去了重新编译和安装的过程。具体如下：

## 升级node.js

**npm中有一个模块叫做 “n”，专门用来管理node.js版本的**。

更新到最新的稳定版只需要在命令行中打下如下代码：

```
# 第一步，安装 n (n 模块不支持 windows 系统)
npm install -g n

# 第二步，升级 node 到最新稳定版本
n stable
```

如需最新版本则用`n latest`

![1496322695012](1496322695012.png)

当然，n后面也可以跟具体的版本号：`n v6.2.0` 或 `n 6.2.0`

node.js升级就是这么简单。

## 升级npm

npm升级就更简单了，只需要在终端中输入：

```
npm -g install npm@next
```



## 补充常用 npm 命令

> 参考 ： https://segmentfault.com/a/1190000006869650

```
$ npm -v          #显示版本，检查npm 是否正确安装。
 
$ npm install express   #安装express模块
 
$ npm install -g express  #全局安装express模块
 
$ npm list         #列出已安装模块
 
$ npm show express     #显示模块详情
 
$ npm update        #升级当前目录下的项目的所有模块
 
$ npm update express    #升级当前目录下的项目的指定模块
 
$ npm update -g express  #升级全局安装的express模块
 
$ npm uninstall express  #删除指定的模块
```





## Windows 强行安装 n 模块（然并软）

> 参考 ： http://blog.csdn.net/u013474104/article/details/52197772

```
PS C:\Users\yafey> npm install -g n
npm ERR! Windows_NT 10.0.14393
npm ERR! argv "C:\\_installed_soft\\nodejs\\node.exe" "C:\\_installed_soft\\nodejs\\node_modules\\npm\\bin\\npm-cli.js" "install" "-g" "n"
npm ERR! node v6.9.2
npm ERR! npm  v3.10.9
npm ERR! code EBADPLATFORM

npm ERR! notsup Unsupported platform for n@2.1.7: wanted {"os":"!win32","arch":"any"} (current: {"os":"win32","arch":"x64"})
npm ERR! notsup Valid OS:    !win32
npm ERR! notsup Valid Arch:  any
npm ERR! notsup Actual OS:   win32
npm ERR! notsup Actual Arch: x64

npm ERR! Please include the following file with any support request:
npm ERR!     C:\Users\yafey\npm-debug.log
```
### 解决方案

在后面加上 `--force` 即可。

```
npm install -g n --force
```



### 然并软

该报错 还是 继续报错。

```
PS C:\Users\yafey> n stable
/bin/bash: C:\Users\yafey\AppData\Roaming\npm\node_modules\n\bin\n: No such file or directory
PS C:\Users\yafey> n v4.4.7
/bin/bash: C:\Users\yafey\AppData\Roaming\npm\node_modules\n\bin\n: No such file or directory
```