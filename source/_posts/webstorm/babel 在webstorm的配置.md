---
title: babel 在webstorm的配置
date: 2022-09-19 14:25:28
tags: ["webstorm","babel"]
categories: ["webstorm"]
---

webstorm监听项目中源代码的变动，利用babel-cli工具进行转译，并将转译好的代码，放到指定的目录中。

## 项目引包（6，7二选一）

(1) babel 6x

```
cnpm install --save-dev babel-cli babel-preset-env
```

(2) babel 7x

```
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

##新增FileWatcher

settings->Tools->File Watchers,点击左下角的+,选择babel


![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/a84d2e37-b9d7-44da-b249-92b050f35f97.webp "")


此时会出现以下对话框，加入信息如下：
（以下为babel 6的信息）

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/c9829cf7-2314-4359-b2a2-9025c9583bde.webp "")


（以下为babel 7的配置）

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/70de7e7f-edff-428c-93e6-51a709955d0c.webp "")

##加入ES6语法支持（并不是必须，只是为了webstorm的语法检查）

此项默认状态下已打开
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/316d1766-86fb-4373-9cae-ca53ee86277c.webp "")
