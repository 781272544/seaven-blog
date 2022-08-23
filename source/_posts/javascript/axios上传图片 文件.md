---
title: axios 上传图片|文件
date: 2022-08-19 14:48:27
tags: ["axios","javascript"]
categories: ["axios"]
---

```javascript
import axios from 'axios';

// 存储文件对象的List
const tempFiles = [图片1, 图片2, 文件1, 文件2];

// 构建FormData对象,通过该对象存储要上传的文件
const formData = new FormData();
// 遍历当前临时文件List,将上传文件添加到FormData对象中
tempFiles.forEach((item) => formData.append('file', item.file));

// 调用后端接口,发送请求
const res = await axios.post('/后台的接口地址', formData, {
  // 因为我们上传了图片,因此需要单独执行请求头的Content-Type
  headers: {
    // 表示上传的是文件,而不是普通的表单数据
    'Content-Type': 'multipart/form-data'
  }
})

```
