---
title: CSS布局：sticky定位
date: 2022-11-10 16:01:16
tags: ["CSS","CSS3","sticky"]
categories: ["CSS3"]
---

stick定位一如其名：它随“正常”文档流而动，直到规定位置，尔后“粘”在那里；或者，当它发现自己可以跟随“正常”文档流而脱离sticky位置时，就果断离开从而加入文档流。

代码与效果如下：
```html
/<div  style="height: 200px; overflow:scroll;">
    <p style="background-color:lightgrey; position:sticky; top: 0px;">This is header A</p>
    <p>This is content A</p>
    <p>This is content A</p>
    <p>This is content A</p>
    <p>This is content A</p>

    <p style="background-color:lightgrey; position:sticky; top: 0px;">This is header B</p>
    <p>This is content B</p>
    <p>This is content B</p>
    <p>This is content B</p>
    <p>This is content B</p>

    <p style="background-color:lightgrey; position:sticky; top: 0px;">This is header C</p>
    <p>This is content C</p>
    <p>This is content C</p>
    <p>This is content C</p>
    <p>This is content C</p>

    <p style="background-color:lightgrey; position:sticky; top: 0px;">This is header D</p>
    <p>This is content D</p>
    <p>This is content D</p>
    <p>This is content D</p>
    <p>This is content D</p>

</div>
```
标题行设置了背景色。如果不设置背景色（背景透明），正常文档流的文字就会和标题行文字重叠在一起显示。

sticky定位的元素会遮住滚动而来的“正常”的文档流；后面的sticky元素会覆盖前面的sticky元素，就好像一层层的便利贴。

