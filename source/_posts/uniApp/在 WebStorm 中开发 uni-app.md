---
title: 在 WebStorm 中开发 uni-app
date: 2022-08-19 16:02:24
tags: ["uniapp","WebStorm"]
categories: ["uniapp"]
---

###全局安装 vue-cli 3.x（如已安装请跳过此步骤）

```
npm install -g @vue/cli
```
###通过 CLI 创建 uni-app 项目

```
vue create -p dcloudio/uni-preset-vue my-project
```

此时，会提示选择项目模板，初次体验建议选择 hello uni-app 项目模板，如下所示：

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/bee07f65-c7f9-47bd-b9cc-135774b1fc59.png "")

###在 WebStorm 中打开项目

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/d54f3a4d-5bbc-4abc-9992-ce7bce046c7d.png "")


###CLI 工程默认带了 uni-app 语法提示和 5+App 语法提示

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/8087f404-60f7-43b4-81ba-7565a13954d7.png "")

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/de425532-75d1-4bae-b7b9-5eb5ae0e7725.png "")

###运行项目

```
npm run dev:%PLATFORM%
```

###发布项目

```
npm run build:%PLATFORM%
```

```%PLATFORM% ```可取值如下：

值 | 平台
--- | :---
h5 | H5
mp-alipay | 支付宝小程序
mp-baidu | 百度小程序
mp-weixin | 微信小程序
mp-toutiao | 头条小程序
mp-qq | qq 小程序


[CLI 方式参考文档](https://uniapp.dcloud.net.cn/quickstart.html#_2-%E9%80%9A%E8%BF%87vue-cli%E5%91%BD%E4%BB%A4%E8%A1%8C "CLI 方式参考文档")

###HBuilderX 工程
HBuilderX 创建的工程默认不带 types 语法提示，在 WebStorm 中编辑的时候，可以自行安装

###初始化npm（如已初始化跳过此步骤）

```
npm init -y
```

###安装 uni-app 语法提示
```
npm i @dcloudio/types -D
```

###注意:
webstorm不识别rpx等单位

如果想了解HBuilderX为uni-app做了什么更好的优化，参考：[https://ask.dcloud.net.cn/article/35451](https://ask.dcloud.net.cn/article/35451)


