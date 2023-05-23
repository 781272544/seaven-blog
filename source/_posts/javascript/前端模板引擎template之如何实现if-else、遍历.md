---
title: 前端模板引擎template之如何实现if-else、遍历
date: 2022-08-19 14:48:27
tags: ["template","模板引擎"]
categories: ["javascript"]
---

###1、先来了解一下template模板

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<div id="app"></div>

<!--第2步：定义模块-->
<!--模板中就可以使用数据-->
<!--type="text/javascript"  表示script标签中只能写JS代码-->
<!--type="text/html" 表示在script标签中写html代码-->
<!-- 如果说，你在script标签中写html代码，整个script标签叫模板-->
<!--模板并不会直接在页面上渲染出来-->
<script type="text/html" id="my">
    <!--在模板中使用if else-->
    <ul>
        {{each news}}
        <li>
            <h3>{{ $value.title }}</h3>
            <p>{{ $value.summary }}</p>
        </li>
        {{/each}}
    </ul>
</script>

<!--第1步：导入模板引擎JS文件-->
<script src="../public/template-web.js"></script>

<script>
    let data = {// 模拟通过ajax获取的数据
        news:[
            {id:1,title:"新闻标题1",summary:"新闻的描述1"},
            {id:2,title:"新闻标题2",summary:"新闻的描述2"},
            {id:3,title:"新闻标题3",summary:"新闻的描述3"},
            {id:4,title:"新闻标题4",summary:"新闻的描述4"},
            {id:5,title:"新闻标题5",summary:"新闻的描述5"}
        ]
    }

    /* 第1个参数是指定模板  template("my",data) 把模板和数据进行柔和*/
    let htmlStr = template("my",data)
    document.getElementById("app").innerHTML = htmlStr;

    let str = ``;
    data.news.forEach(item=>{
        str += `
            <li>
                <h3>${ item.title }</h3>
                <p>${ item.summary }</p>
            </li>
        `
    })
</script>
</body>
</html>
```
###2、如何在模板中实现if-else
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<div id="app"></div>

<!--模板中就可以使用数据-->
<script type="text/html" id="my">
    <!--在模板中使用if else-->
    <ul>
        {{if score>=60}}
            <li>及格</li>
        {{else}}
            <li>不及格</li>
        {{/if}}
    </ul>
</script>

<script src="../public/template-web.js"></script>
<script>
    let data = {
        score:89
    }
    let htmlStr = template("my",data)
    document.getElementById("app").innerHTML = htmlStr;
</script>
</body>
</html>
```
###3、如何在模板中实现遍历
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<div id="app"></div>

<!--模板中就可以使用数据-->
<script type="text/html" id="my">
    <!--在模板中使用if else-->
    <ul>
        {{each news}}
            <li>
                <h3>{{ $value.title }}</h3>
                <p>{{ $value.summary }}</p>
            </li>
        {{/each}}
    </ul>
</script>

<script src="../public/template-web.js"></script>
<script>
    let data = {
        news:[
            {id:1,title:"新闻标题1",summary:"新闻的描述1"},
            {id:2,title:"新闻标题2",summary:"新闻的描述2"},
            {id:3,title:"新闻标题3",summary:"新闻的描述3"},
            {id:4,title:"新闻标题4",summary:"新闻的描述4"},
            {id:5,title:"新闻标题5",summary:"新闻的描述5"}
        ]
    }
    let htmlStr = template("my",data)
    document.getElementById("app").innerHTML = htmlStr;

    let str = ``;
    data.news.forEach(item=>{
        str += `
            <li>
                <h3>${ item.title }</h3>
                <p>${ item.summary }</p>
            </li>
        `
    })
    console.log(str)
</script>
</body>
</html>
```
###4、如何在template中使用@解析html代码
```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
<div id="app"></div>

<!--模板中就可以使用数据-->
<script type="text/html" id="my">
    <ul>
        <li>{{ res }}</li>
        <!-- 如果想解析html代码，可以使用@ -->
        <li>{{@ res }}</li>
    </ul>
</script>

<script src="../public/template-web.js"></script>
<script>
    let data = {
        res:"<strong>hello 模板引擎</strong>"
    }
    let htmlStr = template("my",data)
    document.getElementById("app").innerHTML = htmlStr;
</script>
</body>
</html>
```
###5、列表循环：
```html
{{each children as c index}}
	内置索引：{{index}}
	属性值：{{c.name}}
	属性值：{{c.age}}
{{/each}}
```
###6、自定义方法：
使用template.helper(name,function)注册方法

在模板中，与其他参数一样，用调用

示例：
```html
//模板内调用
{{getAge(age)}}

//自定义函数
template.helper("getAge",function(age){
    console.log(age);
});
```
