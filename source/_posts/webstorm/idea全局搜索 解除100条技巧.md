---
title: idea 全局搜索_IDEA 系列 IDE 解除全局搜索/替换限制100条技巧
date: 2022-08-12 11:10:23
tags: ["webstorm","idea","idea 全局搜索"]
categories: ["webstorm"]
---

如果你在开发中使用的是 IDEA 系列 IDE，譬如 IDEA、Android Studio、CLion、WebStorm、GoLand 等，而且做的是大型项目，你可能经常在查找或者查找替换时觉得挺痛苦，因为搜到的东西往往是下面这样的表现：

## 直接上干货

1、双击你键盘的 shift 键进入如下窗口。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/dc78568e-f0c8-4afd-9d1b-0e4dd32012b5.png "")


2、在如上窗口输入 Registry，然后点击进入，如下所示。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/11a4d249-56ef-4fd5-9ad3-4b056217d843.jpg "")


3、在上面的窗口中找到 ide.usages.page.size 这个配置项，把 value 从 100 改到你想要的大小，然后关闭。这个值就是你搜索时一把显示多少出来的限制值。

4、接着我们现在在项目中搜索，你会发现不再是 100+ 了，而是一把就显示出了所有值。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/1b265658-7659-4e8e-878a-1e8cb117c0a8.jpg "")
