---
title: element ui upload上传实现图片压缩
date: 2022-08-19 14:48:27
tags: ["element ui","图片压缩"]
categories: ["element"]
---

```js
import { compress, compressAccurately } from 'image-conversion'

function handleBeforeUpload(file){
    return new Promise((resolve, reject) => {
        if (file.size / 1024 > 200) { // 大于 200 kb 就压缩
            compressAccurately(file, 200).then(res => {
                resolve(res)
            })
        } else{
            // 无需压缩直接返回原文件
            resolve(file)
        }
        proxy.$modal.loading("正在上传图片，请稍候...");
    })
}
```
