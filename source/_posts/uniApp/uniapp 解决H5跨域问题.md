---
title: uniapp 解决H5跨域问题
date: 2022-08-19 14:48:27
tags: ["uniapp","uniapp 跨域","反向代理"]
categories: ["uniapp"]
---

解决H5跨域问题 找到这里


![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/5deff6e5-6ac5-42f7-85b8-cf7238818d01.png "")


manifest.json文件中，点击“源码视图”,在此对象的最后添加以下代码：

```json
"h5": {
    "devServer": {
        "disableHostCheck": true,
        "proxy": {
            "/api": {
                "target": "http://qcpd.szyqa.com" // 你需要反向代理的域名或ip,
                "changeOrigin": true,
                "secure": false,
                "pathRewrite": {
                    "^/api": "/"
                }
            }
        }
    },
}
```

这是封装的请求头 把baceUrl的值改成"/api" 或者把API_URL全局变量改成"/api" 这是我在env.js里面定义的 。
起初不太理解为什么这样写 这样写是怎么实现的调用的反向代理 你可以这么理解 “在manifest.json的源码视图中的h5下进行如下配置，意味着将uni.request发起网络请求时，碰到的/api字符，将转发到tatget的配置的域名”

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/0a55c303-36d8-4c06-b1ba-332978a2d472.png "")

然后重启服务 有时候需要重启IDE
