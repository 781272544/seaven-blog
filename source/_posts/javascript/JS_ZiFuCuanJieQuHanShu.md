title: JS字符串截取函数
author: 68HTML
date: 2022-04-07 09:13:42
tags: ["Js相关","JavaScript"]
categories: ["前端"]
---
## Bcrypt密码在线生成计算器

> 这篇文章主要介绍了JS字符串截取函数实例，有需要的朋友可以参考一下

使用 substring()或者slice()

函数：split()
功能：使用一个指定的分隔符把一个字符串分割存储到数组
例子：
```javascript
str=”jpg|bmp|gif|ico|png”;
arr=theString.split(”|”);
//arr是一个包含字符值”jpg”、”bmp”、”gif”、”ico”和”png”的数组
```
函数：John()
功能：使用您选择的分隔符将一个数组合并为一个字符串
例子：
```javascript
var delimitedString=myArray.join(delimiter);
var myList=new Array(”jpg”,”bmp”,”gif”,”ico”,”png”);
var portableList=myList.join(”|”);
//结果是jpg|bmp|gif|ico|png
```
函数：substring()
功能：字符串截取，比如想从”MinidxSearchEngine”中得到”Minidx”就要用到substring(0,6)
函数：indexOf()
功能：返回字符串中匹配子串的第一个字符的下标
```javascript
var myString=”JavaScript”;
var w=myString.indexOf(”v”);w will be 2
var x=myString.indexOf(”S”);x will be 4
var y=myString.indexOf(”Script”);y will also be 4
var z=myString.indexOf(”key”);z will be -1
```
