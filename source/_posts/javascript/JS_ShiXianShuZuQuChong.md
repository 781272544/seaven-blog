title: js实现数组去重的方式(7种)
author: 68HTML
date: 2022-04-06 17:17:39
tags: ["JS相关","JavaScript"]
categories: ["前端"]
---
## JS数组去重的方式

例：将下面数组去除重复元素（以多种数据类型为例）

```javascript
const arr = [1, 2, 2, 'abc', 'abc', true, true, false, false, undefined, undefined, NaN, NaN]
```
#### 1.利用Set()+Array.from()

Set对象：是值的集合，你可以按照插入的顺序迭代它的元素。 Set中的元素只会出现一次，即Set中的元素是唯一的。
Array.from() 方法：对一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。
```javascript
const result = Array.from(new Set(arr))
console.log(result) // [ 1, 2, 'abc', true, false, undefined, NaN ]
```
注意：以上去方式对NaN和undefined类型去重也是有效的，是因为NaN和undefined都可以被存储在Set中， NaN之间被视为相同的值（尽管在js中：NaN !== NaN）。
#### 2.利用两层循环+数组的splice方法
> 通过两层循环对数组元素进行逐一比较，然后通过splice方法来删除重复的元素。此方法对NaN是无法进行去重的，因为进行比较时NaN !== NaN。

```javascript
function removeDuplicate(arr) {
  let len = arr.length
  for (let i = 0; i < len; i++) {
    for (let j = i + 1; j < len; j++) {
      if (arr[i] === arr[j]) {
        arr.splice(j, 1)
        len-- // 减少循环次数提高性能
        j-- // 保证j的值自加后不变
      }
    }
  }
  return arr
}

const result = removeDuplicate(arr)
console.log(result) // [ 1, 2, 'abc', true, false, undefined, NaN, NaN ]
```
#### 3.利用数组的indexOf方法
> 新建一个空数组，遍历需要去重的数组，将数组元素存入新数组中，存放前判断数组中是否已经含有当前元素，没有则存入。此方法也无法对NaN去重。

indexOf() 方法：返回调用它的String对象中第一次出现的指定值的索引，从 fromIndex 处进行搜索。如果未找到该值，则返回 -1。

```javascript
function removeDuplicate(arr) {
  const newArr = []
  arr.forEach(item => {
    if (newArr.indexOf(item) === -1) {
      newArr.push(item)
    }
  })
  return newArr // 返回一个新数组
}

const result = removeDuplicate(arr)
console.log(result) // [ 1, 2, 'abc', true, false, undefined, NaN, NaN ]
```
#### 4.利用数组的includes方法
> 此方法逻辑与indexOf方法去重异曲同工，只是用includes方法来判断是否包含重复元素。

includes()方法：用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 true，否则返回 false。
```javascript
function removeDuplicate(arr) {
  const newArr = []
  arr.forEach(item => {
    if (!newArr.includes(item)) {
      newArr.push(item)
    }
  })
  return newArr
}

const result = removeDuplicate(arr)
console.log(result) // [ 1, 2, 'abc', true, false, undefined, NaN ]
```
注意：为什么includes能够检测到数组中包含NaN，其涉及到includes底层的实现。如下图为includes实现的部分代码，在进行判断是否包含某元素时会调用sameValueZero方法进行比较，如果为NaN，则会使用isNaN()进行转化。

具体实现可参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/includes
![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/70833182-7721-44ba-b4ef-c0e82878fc92.png "")
简单测试includes()对NaN的判断：
```javascript
const testArr = [1, 'a', NaN]
console.log(testArr.includes(NaN)) // true
```
#### 5.利用数组的filter()+indexOf()
> filter方法会对满足条件的元素存放到一个新数组中，结合indexOf方法进行判断。

filter() 方法：会创建一个新数组，其包含通过所提供函数实现的测试的所有元素。
```javascript
function removeDuplicate(arr) {
  return arr.filter((item, index) => {
    return arr.indexOf(item) === index
  })
}

const result = removeDuplicate(arr)
console.log(result) // [ 1, 2, 'abc', true, false, undefined ]
```
注意：这里的输出结果中不包含NaN，是因为indexOf()无法对NaN进行判断，即arr.indexOf(item) === index返回结果为false。测试如下：
```javascript
const testArr = [1, 'a', NaN]
console.log(testArr.indexOf(NaN)) // -1
```
#### 6.利用Map()
> Map对象是JavaScript提供的一种数据结构，结构为键值对形式，将数组元素作为map的键存入，然后结合has()和set()方法判断键是否重复。

Map 对象：用于保存键值对，并且能够记住键的原始插入顺序。任何值（对象或者原始值）都可以作为一个键或一个值。
```javascript
function removeDuplicate(arr) {
  const map = new Map()
  const newArr = []

  arr.forEach(item => {
    if (!map.has(item)) { // has()用于判断map是否包为item的属性值
      map.set(item, true) // 使用set()将item设置到map中，并设置其属性值为true
      newArr.push(item)
    }
  })

  return newArr
}

const result = removeDuplicate(arr)
console.log(result) // [ 1, 2, 'abc', true, false, undefined, NaN ]
```
注意：使用Map()也可对NaN去重，原因是Map进行判断时认为NaN是与NaN相等的，剩下所有其它的值是根据 === 运算符的结果判断是否相等。
#### 7.利用对象
> 其实现思想和Map()是差不多的，主要是利用了对象的属性名不可重复这一特性。
```javascript
function removeDuplicate(arr) {
  const newArr = []
  const obj = {}

  arr.forEach(item => {
    if (!obj[item]) {
      newArr.push(item)
      obj[item] = true
    }
  })

  return newArr
}

const result = removeDuplicate(arr)
console.log(result) // [ 1, 2, 'abc', true, false, undefined, NaN ]
```
