---
title: hexo与gitpage搭建个人博客
date: 2019-01-29 18:48:40
tags:
---
如何使用GitHub与HEXO搭建自己的免费博客 2017-12-18
这几天在学习Git版本控制工具，萌生了拥有自己博客的想法，在网上看到很多教程github pages+Hexo+阿里云的教程。今天我结合自己的经验把教程分享给大家。

先在本地装上Node.js与Git
安装Node.js
More info: Node.js安装

安装Git
More info: Git安装

在GitHub官网注册账号
More info: GitHub使用

创建自己的仓库
取名为 username.github.io
username为自己的GitHub ID

下载HEXO
``` bash
$ cd d:/hexo
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
```
$ hexo g # 或者hexo generate
$ hexo s # 或者hexo server，可以在http://localhost:4000/ 查看
增加新文章
``` bash
$ hexo new "title"
```
将hexo生成的public目录的文件push到仓库
打开命令行

``` bash
$ git init
$ git add .
$ git commit
$ git push origin master
```
现在就可以通过username.github.io访问了

在阿里云购买域名
购买能够备案的域名

域名解析选择CNAME
在本地创建CNAME无后缀名文件，添加上你买的域名如xxx.net
不要加www或者http

到此博客搭建成功了，感觉不错给我糖吃吧