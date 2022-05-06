---
title: 微信小程序中使用vant框架的具体步骤
date: 2022-04-07 10:13:41
tags: ["微信小程序", "Vant"]
categories: ["微信小程序"]
---

> 本文主要介绍了微信小程序中使用vant框架的具体步骤，文中通过示例代码介绍的非常详细，具有一定的参考价值，感兴趣的小伙伴们可以参考一下

1.说到vant框架相信大家应该并不陌生了吧，做过移动端开发的小伙伴们应该都知道它吧。

2.Vant 是有赞前端团队开源的移动端组件库，于 2017 年开源，已持续维护 4 年时间。Vant 对内承载了有赞所有核心业务，对外服务十多万开发者，是业界主流的移动端组件库之一。

3.我们废话不多说，直接进入今天的主题。我们该如何在微信小程序中去使用vant组件库呢！

首先

我们先打开vant weapp网站，这里我将网站地址给大家。Vant Weapp 网址

大家打开网站后呢，点击快速上手。上面就有步骤教你如何在小程序中使用vant组件库。

下面呢就给大家介绍一下我是如何去安装使用vant UI组件库的。

## 1.打开我们小程序的项目目录，然后打开文件所在的位置。
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/3a6bba8a-eedd-4d9b-9016-b08dbaf88be8.jpg "")
## 2.初始化项目文件
这里呢我通过 cmd 窗口初始化
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/edcb41c1-9cb9-4846-b31c-56a658dee28c.jpg "")
## 3.输入初始化项目的命令 
```
npm init
```
此时你会发现你的目录多出了package.json文件
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/619fe8db-1d60-416f-a7e9-4f7bdd922459.jpg "")
## 4.安装依赖 
4.1 通过 npm 安装vant/weapp
```
npm i @vant/weapp -S --production
```
4.2 安装 miniprogram
```
npm i miniprogram-sm-crypto --production
```
安装完毕后，你会发现你的目录中又多些文件。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/fa52cacf-d96f-44dc-a4bb-332e942ec79b.jpg "")
4.3 修改 app.json

将 app.json 中的 "style": "v2" 去除，原因是小程序的新版基础组件强行加上了许多样式，难以覆盖，不关闭将造成部分组件样式混乱。

4.4 修改 project.config.json

开发者工具创建的项目，miniprogramRoot 默认为 miniprogram，package.json 在其外部，npm 构建无法正常工作。需要手动在 project.config.json 内添加如下配置，使开发者工具可以正确索引到 npm 依赖的位置。
```
{
  ...
  "setting": {
    ...
    "packNpmManually": true,
    "packNpmRelationList": [
      {
        "packageJsonPath": "./package.json",
        "miniprogramNpmDistDir": "./miniprogram/"
      }
    ]
  }
}
```
4.5 构建 npm 我们点击左上角的工具栏
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/d7bf3be1-ab82-457f-8182-05d12939e315.jpg "")
构建成功后会出现下面的画面

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/a96df185-2cf1-4518-ab52-cf747462ee3c.jpg "")

4.6然后点击右上角的详情---本地设置----使用npm模块

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/81b8c4f3-35f6-4d2c-a1fe-dfcf7867c523.jpg "")
## 5.使用组件
我这里在全局里面注册一个按钮，然后使用它。先去app.json中注册
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/5a03d6cb-7582-4a02-94ca-2fc5f98b1ca2.jpg "")

这里我随便找一个页面用一下这个按钮组件。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/46289e16-c4d9-4e18-9773-4769c049b6e2.jpg "")
大家可以看到我使用成功了。

> 到此这篇关于微信小程序中使用vant框架的具体步骤的文章就介绍到这了,更多相关小程序使用vant框架内容请搜索68HTML以前的文章或继续浏览下面的相关文章希望大家以后多多支持68HTML！

