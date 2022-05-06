title: JS 闭包
author: Seaven
date: 2022-04-06 17:36:52
tags: ["JavaScript"]
categories: ["前端"]
---
## JS 闭包

> 如果一个函数能访问另一个函数中的变量，则这个函数就称为闭包。最直接的是：函数a中定义了一个函数b，且在函数a外能够调用这个函数b，就会形成闭包。

注意：闭包只能取得包含函数的变量的最后一个值，如:
```javascript
function getButton(){
      for(var i=0;i<3;i++){
          var element=document.createElement("button");
          var elem_text=document.createTextNode("Button"+i);
          element.appendChild(elem_text);
          document.body.appendChild(element);

          element.onclick=function(){    //闭包
              alert(i);
          };
      }
  }
getButton();
```
点击按钮时弹出的警告框的值都为3.将代码做如下修改：
```javascript
function getButton(){
      for(var i=0;i<3;i++){
          var element=document.createElement("button");
          var elem_text=document.createTextNode("Button"+i);
          element.appendChild(elem_text);
          document.body.appendChild(element);

          element.onclick=(function(num){
              return function(){
                  alert(num);
              };
          })(i);  //强制将参数i传递进去并立即调用执行
      }
  }
  getButton();
```
当然有多种方法，将红色部分也可改为：
```javascript
(function(num){element.onclick=function(){   
  alert(num);
 };  //这样也可以
})(i);
```
再举个例子：
```javascript
var arr = ['第一次','第二次','第三次'];
for(var i=0;i<arr.length;i++){
  setTimeout(function(){
     document.getElementById('info').innerHTML = arr[i];
    },i*1000);
}
```
则三次输出结果都为undefined，原因同样是闭包只能取得包含函数的变量中的最后一个值。for循环执行完毕后，i的值为3，而arr[3]=undefined;对函数进行修改如下：
```javascript
var arr = ['第一次','第二次','第三次'];

for(var i=0;i<arr.length;i++){
  (function(num){
     setTimeout(function(){
          document.getElementById('info').innerHTML = arr[i];
     },i*1000);
  })(i);
}
```
结果，刚打开页面时显示"第一次"，再过1秒显示“第二次”，再过2秒显示“第三次”。
