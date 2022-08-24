---
title: Promise构造函数的方法1:Promise.resolve()和Promsie.reject()
date: 2022-08-24 09:43:56
tags: ["Promise","javascript"]
categories: ["javascript"]
---

##1、promise.resolve()
它是成功状态的Promsie的一种简写方式。

####参数传递：（以下重点掌握一般参数的传递）：
1）一般参数和参数为空的情况下，直接返回一个resolved状态的Promise对象；

2）如果参数是Promise的实例，将不做任何修改、原封不动地返回这个实例；

3）如果参数是具有then方法的对象，Promise.resolve()会将这个对象转为Promise对象，并立即执行这个对象的then方法。

ps: 重视返回的Promise的状态对其后者执行的函数的影响

```javascript
    // Promise.resolve()
    // 1.Promise.resolve()的本质
    // 它是成功状态的Promsie的一种简写方式
    //new Promise(resolve => resolve('foo'));
    // 简写
    //Promise.resolve('foo');

    // 2.参数
    // 2.1一般参数的传递:（重点）
    // 调用Promise.resolve()方法传递的一般参数原封不动地向后传递，由then的第一个处理函数的形参接收
    Promise.resolve('foo').then(data => {
    console.log(data); //输出 foo
})

    // 2.2 Promise.resolve()传递一个Promise作为参数：（了解）
    //当Promise.resolve()接收的是Promise对象时，直接返回这个Promise对象，什么都不做
    const p1 = new Promise(resolve => {
    setTimeout(resolve,1000,'我执行了');
    // 上面这句代码等同于：
    // setTimeout(()=>{
    //     resolve("我执行了");
    // },1000);
    // ps:也就是说setTimeout的第三个参数是传递到它的处理函数中做参数的
})
    Promise.resolve(p1).then(data => {
    console.log(data); //一秒后输出：我执行了
});
    // 等价于：
    p1.then(data => {
    console.log(data); //一秒后输出：我执行了
})
    console.log(Promise.resolve(p1) === p1); //输出true

    // 我们可以衍生出：
    // 当resolve函数接收的是Promise对象时，后面的then会根据传递的Promise对象的状态变化决定执行哪一个回调
    new Promise(resolve => resolve(p1)).then(data => {
    console.log(data); //一秒后输出：我执行了
})

```
##2.Promise.reject()
####它是失败状态的Promsie的一种简写方式：
new Promise(reject => reject('reason'));

简写：Promise.reject('reason');
####参数传递：
不管什么参数，都会原封不动地向后传递，作为后续方法的参数。

####扩展：
学完这两个方法以后，我们返回一个错误的Promise变得更简便：

```javascript
new Promise((resolve,reject)=>{
    resolve(123)
})
.then(data => {
    console.log(data);
    //返回一个失败状态的Promise:
    return Promise.reject('reason');
})
.catch(err => console.log(err));
```
