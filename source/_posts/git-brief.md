---
title: git 常用操作
tags:
  - git
categories: git
typora-copy-images-to: git-brief
typora-root-url: git-brief
date: 2017-06-06 22:15:58
---



# git 初始化（以下命令在 Git Bash中操作）



## 添加 Coding 账户 SSH 公钥 `TL;DR` [^1]

> 参考 ： [coding 帮助中心 <SSH公钥配置>](https://coding.net/help/doc/account/ssh-key.html)

### 生成公钥

打开命令行终端输入`ssh-keygen -t rsa -C “username@example.com”`,( 注册的邮箱)，接下来点击enter键即可（也可以输入密码）。

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
# Generating public/private rsa key pair.
Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]  // 因为有多个git 仓库，所以 推荐跟仓库名及使用的电脑相关， 如：coding_pc
Enter passphrase (empty for no passphrase):
```

成功之后

```
Your identification has been saved in /Users/you/.ssh/id_rsa.
# Your public key has been saved in /Users/you/.ssh/id_rsa.pub.
# The key fingerprint is:
# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```

### 添加公钥

1. 本地打开 id_rsa.pub 文件，复制其中全部内容，填写到SSH_RSA公钥key下的一栏，公钥名称可以随意起名字。
2. 完成后点击“添加”，然后输入密码或动态码即可添加完成。

完成后在命令行测试，首次建立链接会要求信任主机。

`ssh -T git@git.coding.net`

```
~/git $ ssh -T git@git.coding.net
Hello yafey! You've connected to Coding.net via SSH successfully!
```

![1496762321975](1496762321975.png)



### 【知识点】多个账户配置 （如 github + coding）

如果需要使用多个账户建议生成多个公钥，可以在 `~/.ssh/config`文件中加上下边一段

```
# 该文件用于配置私钥对应的服务器
Host git.coding.net
User xxxx@email.com   # 这里 也可以配置成 `User git`
PreferredAuthentications publickey
IdentityFile ~/.ssh/coding_rsa   # 生成的非默认地址的公钥存放点
```





### `git clone --recursive`

好像是会把 submodule 也更新好， 待验证。



## git 初始化项目

在命令行中创建 Git 仓库

```
mkdir HexoBlogSource
cd HexoBlogSource
git init
echo "# HexoBlogSource" >> README.md
git add README.md
git commit -m "first commit"
git remote add origin https://git.coding.net/yafey/HexoBlogSource.git
git push -u origin master
```




已有项目

```
git remote add origin https://git.coding.net/yafey/HexoBlogSource.git
git push -u origin master
```





# git 常用操作集锦

## tag remote branch HEAD directly

> 参考 ： [Git: Is there a way to tag remote branch HEAD directly by commit id? ](https://stackoverflow.com/questions/41129817/git-is-there-a-way-to-tag-remote-branch-head-directly-by-commit-id)

**No real need to mess with local tags**, just do the rewrite while pushing.

```
# 替换下面的 <> 中的内容， 如果只有一个 remote ，origin 不用替换。
git push <origin> refs/remotes/<origin>/<branch1>:refs/tags/<tagged-branch1>
```



[^1]: `Too Long; Don’t Read`的缩写，换成中国话，就是**`太长了，读不下去`**的意思。
> > [网络上的文章都太长了？ @2012-11-27 09:30](http://www.ifanr.com/204080)
>
> 根据字典解释，“TL;DR”发源自网络。一般用于回复别人写得太多，反而让人抓不住重点，**现在有的人把它放在一段话的开头，代表“`长话短说`”的意思**——如今不少服务都直接以“TL;DR”为命名，把网络上一篇很长的报道，缩短成简单的三两句话，让人快速抓住文章的精华。