title: Nuxt.js 报错 window is not defined || document is not defined
author: Seaven
date: 2022-04-06 12:22:13
tags: ["Nuxt","NuxtJs报错"]
categories: ["Nuxt"]
---

## Nuxt.js 报错 window is not defined || document is not defined
### 情况1： 自己的写的函数里包含window等
报错原因：因为Nuxt为服务器端渲染，所以在编译打包时会区分服务端渲染还是客户端渲染(浏览器)，在vue文件中使用window对象报错的原因是，webpack将其加入了服务端脚本中，所以会报错。所以在使用时，应该判断当前代码环境是否是浏览器环境。

解决方案:
> 1.通过 process.client 判断
```
if (process.client) {
  ... // 这里就是操作window对象的代码
}
```
> 2.将涉及到window的写在 mounted 生命周期里
```
mounted() {
	// window ...
}
```
> 3.使用 no-ssr 组件
```
<template>
    <div>
        <kafuuchino/>
        <no-ssrplaceholder="Loading...">
            <!-- 此组件仅在客户端呈现 -->
            <comments/>
        </no-ssr>
    </div>
</template>
```
### 情况2： 第三方插件里包含window等
报错原因：还有一种就是项目里会引入很多第三方组件，这些组件里也有可能会包含window等一些服务端不支持的内容

解决方案:
>1.将插件设置为客户端渲染
将 插件 文件路径配置到 nuxt.config.js 的 plugins 属性中，示例如下

```
module.exports = {
	 //其它配置项...
	plugins: [
	    { 
	    	src: '~/plugins/kafuuchino',
	    	ssr: false // 此处的 ssr:false 就是将其改为非服务器端渲染
	    } 
	],
}
```
修改完配置文件需要重新启动项目！
