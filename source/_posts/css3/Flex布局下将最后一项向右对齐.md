---
title: Flex布局下将最后一项向右对齐
date: 2022-09-13 11:26:36
tags: ["CSS","CSS3","flex"]
categories: ["CSS3"]
---

在使用display:flex的前提下，对项目的最后一个元素进行向右对齐，则使用margin-left:auto(在最后一个元素)即可。大概就是下面的这个意思：

```html
<div class="animal">
    <div class="dog"></div>
    <div class="cat"></div>
    <div class="big"></div>
</div>
```

```css
.animal {
    display: flex;
}
.dog{

}
.cat{

}
.big{
    mergin-left: auto;
}
```
如果只是两个元素进行对齐就可以直接在父元素那里加上justity-content:space-between;即可。

