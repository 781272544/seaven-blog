title: 微信小程序中wxs文件的一些妙用分享
author: 68HTML
date: 2022-04-07 09:19:55
tags: ["微信小程序","wxs"]
categories: ["微信小程序"]
---
## 前言
wxs文件是小程序中的逻辑文件，它和wxml结合使用。

不同于js， wxs可以直接作用到视图层，而不需要进行视图层和逻辑层的setData数据交互；

因为这个特性，wxs非常适合应用于优化小程序的频繁交互操作中；
## 应用
### 过滤器
在IOS环境中wxs的运行速度要远高于js，在android中两者表现相当。

使用wxs作为过滤器也可以一定幅度提升性能；让我们来看一个过滤器来了解其语法。

wxs文件:
```
var toDecimal2 = function (x) {
  var f = parseFloat(x);
  if (isNaN(f)) {
    return '0.00'
  }
  var f = Math.round(x * 100) / 100;
  var s = f.toString();
  var rs = s.indexOf('.');
  if (rs < 0) {
    rs = s.length;
    s += '.';
  }
  while (s.length <= rs + 2) {
    s += '0';
  }
  return s;
}
module.exports = toDecimal2
```
上面的代码实现了数字保留两位小数的功能。

wxml文件：
```
<wxs src="./filter.wxs" module="filter"></wxs>
<text>{{filter(1)}}</text>
```
基本语法：在视图文件中通过wxs标签引入，module值是自定义命名，之后在wxml中可以通过filter调用方法

上面的代码展示了 wxs的运行逻辑，让我们可以像函数一样调用wxs中的方法；

下面再看一下wxs针对wxml页面事件中的表现。
### 拖拽
使用交互时（拖拽、上下滑动、左右侧滑等）如果依靠js逻辑层，会需要大量、频繁的数据通信。卡顿是不可避免的；

使用wxs文件替代交互，不需要频繁使用setData导致实时大量的数据通信,从而节省性能。

下面展示一个拖拽例子

wxs文件：
```
function touchstart(event) {
  var touch = event.touches[0] || event.changedTouches[0]
  startX = touch.pageX
  startY = touch.pageY
}
```
事件参数event和js中的事件event内容中touches和changedTouches属性一致
```
function touchmove(event, ins) {
  var touch = event.touches[0] || event.changedTouches[0]
  ins.selectComponent('.div').setStyle({
    left: startX - touch.pageX + 'px',
    top: startY - touch.pageY  + 'px'
  })
}
```
ins(第二个参数)为触发事件的视图层wxml上下文。可以查找页面所有元素并设置style,class(足够完成交互效果)

注意：在参数event中同样有一个上下文实例instance；event中的实例instance作用范围是触发事件的元素内，而事件的ins参数作用范围是触发事件的组件内。
```
module.exports = {
  touchstart: touchstart,
  touchmove: touchmove,
}
```
最后将方法抛出去，给wxml文件引用。

wxml文件
```
<wxs module="action" src="./movable.wxs"></wxs> 
<view class="div" bindtouchstart="{{action.touchstart}}" bindtouchmove="{{action.touchmove}}"></view>
```
上面的例子，解释了事件的基本交互用法。
### 文件之中相互传参
在事件交互中，少不了需要各个文件之中传递参数。 下面是比较常用的几种
### wxs传参到js逻辑层
wxs文件中：
```
var dragStart = function (e, ins) {
    ins.callMethod('callback','sldkfj')
}
```
js文件中：
```
callback(e){
    console.log(e)
}
// sldkfj
```
使用callMethod方法，可以执行js中的callback方法。也可以实现传参；

！！！callMethod不支持传回调函数*
### js逻辑层传参到wxs文件
js文件中：
```
handler(e){
    this.setData({a:1})
}
```
wxml文件：
```
<wxs module="action" src="./movable.wxs"></wxs> 
<view change:prop="{{action.change}}" prop="{{a}}"></view>
```
wxs文件中：
```
change(newValue,oldValue){}
```
js文件中的参数传递到wxs需要通过wxml文件中转。

js文件触发handler事件，改变a的值之后，最新的a传递到wxml中。

wxml中prop改变会触发wxs中的change事件。change中则会接收到最新prop值
### wxs中获取dataset(wxs中获取wxml数据）
wxs中代码
```javascript
var dragStart = function (e) {
  var index = e.currentTarget.dataset.index;
  var index = e.instance.getDataset().index;
}
```
上面有提到e.instance是当前触发事件的元素实例。

所以e.instance.getDataset()获取的是当前触发事件的dataset数据集
### 注意点
wxs和js为不同的两个脚本语言。但是语法和es5基本相同，确又不支持es6语法； getState 在多元素交互中非常实用，欢迎探索。

不知道是否是支持的语法可以跳转官网文档； [wxs运算符、语句、基础类库、数据类型](https://developers.weixin.qq.com/miniprogram/dev/reference/wxs/05statement.html "wxs运算符、语句、基础类库、数据类型")
## 总结
到此这篇关于微信小程序中wxs文件的一些妙用的文章就介绍到这了,更多相关微信小程序wxs文件妙用内容请搜索68HTML以前的文章或继续浏览下面的相关文章希望大家以后多多支持68HTML！
