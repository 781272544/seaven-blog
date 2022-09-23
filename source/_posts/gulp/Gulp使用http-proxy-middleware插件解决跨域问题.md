---
title: Gulp使用http-proxy-middleware插件解决跨域问题
date: 2022-09-20 14:23:46
tags: ["gulp","Gulp使用http-proxy-middleware","跨域""]
categories: ["gulp"]
---

查阅相关资料后可以使用http-proxy-middleware 模块进行代理，其官方说明文档也比较详细,我使用是gulp-connect作为web服务器，因此在此基础上，只需要增加对的代理中间件的相关配置即可，下面是我的Gulp配置
```js
const { createProxyMiddleware } = require('http-proxy-middleware');  // 1.0.0版本的引用方式,不然会报错

function server() {
    $.connect.server({
        host: internalIp.v4.sync(),
        root: "./dist",
        port: '2222',
        index: false, // 默认不打开首页
        livereload: true,
       // debug: true 
        middleware: function (connect, opt) {
            return [
                createProxyMiddleware('/action', {
                    target: 'http://condor2400.startdedicated.com/action',//代理的目标地址
                    changeOrigin: true,//
                    pathRewrite: {//路径重写规则 
                        '^/action': ''
                    }
                })
            ]
        }
    });
}
```
