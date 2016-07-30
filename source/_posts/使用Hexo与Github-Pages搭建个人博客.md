---
title: 使用Hexo与Github Pages搭建个人博客
date: 2016-07-27 11:15:28
tags: 
- Github
- Hexo
categories:
- Hexo
---

## 什么是Hexo?

Hexo是一个快速、简介且高效的博客框架。Hexo使用Markdown（或其他渲染引擎）解析文章，
在几秒内，即可利用靓丽的主题生成静态网页

## 安装前提

安装Hexo之前，需要安装下列程序
+ Node.js
+ Git

## 安装Hexo

> $ npm install -g hexo-cli

## 快速体验Hexo

+ 初始化Hexo文件夹
> $ hexo init &lt;folder> && cd &lt;folder>

+ 生成静态页面并预览
> $ hexo g && hexo s

+ 在浏览器中输入`localhost:4000`，一个Hexo站点就展示在你面前了

<!-- more --> 

## 目录结构
```
.
├── _config.yml
├── package.json
├── scaffolds
├── scripts
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
+ _config.yml : 配置文件，主要配置自己的站点信息，配置文件中注释非常详细，
如果不懂的话，可以去Hexo官网文档中查看（Hexo是台湾人做的，对中文友好）

+ scaffolds : 脚手架，大概是一些插件什么的，先不用管

+ source/_drafts : 草稿文件目录

+ source/_posts : 发表的博文目录

+ themes : 下载的主题目录。更多的主题，只需要git clone到themes中，然后修改
config.yml中的theme字段就可以了，十分方便

其它目录先不用管

## 常用指令

```
$ hexo new [layout] <title>   #建立新文章，默认在_posts下，layout="draft"时发布的是草稿
$ hexo public <filename>      #将_drafts下的文件放到_posts下，发布草稿
$ hexo generate               #生成静态网页
$ hexo server                 #启动预览服务器，开启-d选项预览草稿
$ hexo deploy                 #发布到远程服务器，开启--generate选项可以在deploy前自动generate
```
简化版
```
$ hexo n  # == hexo new
$ hexo p  # == hexo publish
$ hexo g  # == hexo generate
$ hexo s  # == hexo server
$ hexo d  # == hexo deploy
```

## 第一个博客
文章头部信息，以`---`结尾
```
title: 我的第一篇博客
layout: post
date: 2016-4-13
comments: true
categories: Blog
tags: [blog]
keywords: Hexo, blog
description: Hexo上第一篇博客
---
```
接下来使用Markdown编辑文章就可以了

## 部署到GitHub Pages上
+ 更改`_config.yml`中的配置
```
deploy:
    type: git
    repo: <repository url>
    branch: [branch]
    message: [message]
```

+ 运行`$ npm install hexo-deployer-git --save`，安装git的deployer

+ 运行`$ hexo g -d`

+ 部署完成，用浏览器打开`http://username.github.io`

## GitHub Pages绑定独立域名

+ 在source中添加`CNAME`文件

+ 在`CNAME`中写入独立域名，比如我的域名
```
ooho.me
```

+ 在`dnspod`(或其他DNS服务商)中添加记录，选择别名记录`CNAME`，主机值选`@`
(或别名记录`CNAME`，主机值选`wwww`)，在记录值中写上自己的github pages域名，
比如`hughoo.github.io`

+ 在`GoDaddy`上设置自己的独立域名的DNS服务器IP(比如dnspod服务器的IP)

+ 运行
```
$ hexo g -d
```


