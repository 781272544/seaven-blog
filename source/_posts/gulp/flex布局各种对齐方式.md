---
title: gulp-file-include 踩坑，不支持else 语法
date: 2022-09-20 14:23:46
tags: ["gulp","gulp-file-include"]
categories: ["gulp"]
---

##gulp-file-include 不支持 else 语法
查阅官方文档，发现是支持 if 语法的：

```
@@include('some.html', {"nav": true})

@@if (name === 'test' && nav ===true) {
    @@include('test.html')
}
```

在实际使用中，自然而然的就用上了 if/else：

```
@@include('test.html', {
    "age": 16
})
```

```
@@if(age >= 18){
    <b>允许进入</b>
}else{
    <b>禁止进入</b>
}
```

但输出的结果是：

> else{ 禁止进入 }

所以这里只能写两个 if 来实现：

```
@@if(age >= 18){
    <b>允许进入</b>
}
@@if(age < 18){
    <b>禁止进入</b>
}
```

如果有更多条件判断，则继续增加 if 语句即可。

####临时变量无法在 if/for/loop 嵌套中使用
这个描述可能不太好理解，具体看下这段代码：

```
@@for(var i = 0; i < arr.length; i++){
    @@if(i == 0){
        <b>这是第一条</b>
    }
}
```

我希望循环 arr 数组的时候，第一条输出“这是第一条”文字，但执行会提示 i is not defined: (i == 0) ，试了好多写法都不行，最后发现用反单引号（`）加三元运算符可以解决这个问题：

```
@@for(var i = 0; i < arr.length; i++){
    `+(i == 0 ? '<b>这是第一条</b>' : '')+`
}
```
