---
title: Hexo程序archive页面数量设置
date: 2022-04-07 10:43:23
tags: ["Hexo"]
categories: ["Hexo"]
---

使用Hexo搭建博客已经有一段时间了，当文章数量达到十几篇左右时，突然发觉archive归档页面仅显示10篇文章，并且出现了分页功能，对于我们这种个人博客，文章数量不会很多，所以更希望是在一页中完全展示出来，便于访问者查找感兴趣的文章。

在网上查找原因，发现此处的10条限制来自_config.yml文件中的配置，这个配置控制所有的分页配置，包括首页、归档页、tag分类页面。

```
per_page: 10
```
如果我们想对上面三个页面做独立的配置，需要安装插件进行功能支持。

+ hexo-generator-index
+ hexo-generator-archive
+ hexo-generator-tag

使用如下命令进行安装需要的插件

```
$ npm install hexo-generator-archive --save
```
对应的_config.yml文件中添加如下配置

```
index_generator:
  per_page: 5

archive_generator:
  per_page: 20  //为0时表示不分页全展示
  yearly: true  //按年生成归档
  monthly: true //按月生成归档

tag_generator:
  per_page: 10
```
> 注意:上面归档设置中的按年或者按月，需要修改模板给出对应的链接入口，对于没有兴趣修改模板的同学，可以将此处设为false，减少生成页面时的工作量。
