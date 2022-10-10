---
title: babel不识别对象展开运算符时报错
date: 2022-10-10 10:15:46
tags: ["gulp","gulp-babel","babel"]
categories: ["gulp"]
---

对象展开运算符时需要安装一个插件

```
npm install --save-dev babel-plugin-transform-object-rest-spread
```

然后在 .babelrc文件中配置

```
{
	"presets": [
	   ["es2015","stage-3"]
	],
	"plugins":["transform-object-rest-spread"]
}
```
