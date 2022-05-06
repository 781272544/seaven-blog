---
title: Hexo博客修改Archive页面显示文章数量
date: 2022-04-07 10:40:55
tags: ["Hexo"]
categories: ["Hexo"]
---

> 之前配置的Swiftype站内搜索功能很不稳定，经常因为网络问题无法返回搜索结果，所以要找写过的某篇文章就不太方便。为解决这个问题，有一个方法是在Archive页面上不分页，然后就可以用浏览器自带的搜索功能来搜索标题了。

默认情况下，Hexo无法对主页、Archive页面、标签页面每页显示文章数量进行单独设置，所以需要安装hexo-generator-archive插件来实现这个功能。

使用如下命令安装：
```
npm install hexo-generator-archive --save
```
安装好后修改_config.yml中的相关配置，分别对index、archive、tag及category页面进行设置即可：
```
# Pagination
## Set per_page to 0 to disable pagination
index_generator:
  per_page: 6

archive_generator:
  per_page: 0

tag_generator:
  per_page: 0

category_generator:
  per_page: 50
```
