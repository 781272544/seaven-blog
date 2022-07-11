---
title: vue select change事件传递自定义参数
date: 2022-07-11 08:44:29
tags: ["vue","elementUI","element select"]
categories: ["element-ui"]
---

> 今天记录一个小问题，最近get到的一个方法，不太常用，记录一下，增强记忆吧。

> 之前在vue项目中，也经常使用select标签，也经常用change事件，经常用的change事件中，一直有个默认参数，就是选中的选项的信息，最近一个需求除了需了选中项信息外，还需要其他的参数，今天记录一下这种传参方式，直接上代码。


```vue

// 普通用法，没有自定义参数
@change="handleChange"

handleChange (event) {
	// event.target.value 是change事件默认传的信息
	console.log(event.target.value)
}

// 需要传自定义参数时
@change="handleChange($event, '自定义参数')"

handleChange (event, param) {
	// event.target.value 是change事件默认传的信息
	// param 是自定义的参数
}
```

