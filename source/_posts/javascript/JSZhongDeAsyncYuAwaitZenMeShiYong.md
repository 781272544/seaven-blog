---
title: JS中的async与await怎么使用
date: 2022-04-07 11:00:23
tags: ["javaScript", "async", "await"]
categories: ["javaScript"]
---

> 这篇文章主要介绍了JS的async/await怎么使用,简单来说，async/await是基于promises的语法糖，使异步代码更易于编写和阅读,下面来看详细的介绍内容吧。需要的小伙伴也可以参考一下

## 一、async
async创建一个异步函数来定义一个代码块，在其中运行异步代码;

怎样变成异步函数呢？以 async 这个关键字开始，它可以被放置在一个函数前面
```javascript
async function f() {
  return 1;
}
  
f().then(alert); // 1
  
//上下结果一样
  
async function f() {
  return Promise.resolve(1);
}
  
f().then(alert); // 1
  
//也可以用箭头函数
let hello = async () => { return "1" };
hello().then((value) => console.log(value))
//返回值也可以简化成这样
hello().then(console.log)
```
异步函数的特征之一：保证函数的返回值为 promise。

将 async 关键字加到函数申明中，可以告诉它们返回的是 promise，而不是直接返回值。此外，它避免了同步函数为支持使用 await 带来的任何潜在开销。
## 二、await:
await 只在异步函数里面才起作用。它可以放在任何异步的，关键字 await 让 JavaScript 引擎等待直到 promise 完成并返回结果。在等待promise的同时，其他正在等待执行的代码就有机会执行了。

您可以在调用任何返回Promise的函数时使用 await，包括Web API函数。
```javascript
async function f() {
  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("咚!"), 1000)
  });
  
  let result = await promise; // 等待执行，直到 promise resolve 执行完
  
  alert(result); // "咚!"
}
  
f();//拿到 result 作为结果继续往下执行。所以上面这段代码在1秒后显示 “咚!”。

```
> 注意：await 实际上会暂停函数的执行，直到 promise 状态变为 完成，然后以 promise 的结果继续执行。这个行为不会耗费任何 CPU 资源，因为 JavaScript 引擎可以同时处理其他任务：执行其他脚本，处理事件等。

## 三、综合应用
有了async/await就去除了到处都是 .then() 代码块，因为await会等待了。

```javascript
async function A() {
  let response = await fetch('c.jpg');
  let myBlob = await response.blob();
  
  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}
  
A()
.catch(e => {
  console.log('问题: ' + e.message);
});
```
用更少的.then()块来封装代码，同时它看起来很像同步代码，所以它非常直观。这样用的很爽！

到此这篇关于JS的async/await怎么使用的文章就介绍到这了,更多相关JS的async/await 用法内容请搜索68HTML以前的文章或继续浏览下面的相关文章希望大家以后多多支持68HTML！
