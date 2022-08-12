---
title: el-table 横向滚动条固定在可视窗口底部
date: 2022-08-12 08:59:33
tags: ["vue","elementUI","element tabel"]
categories: ["element-ui"]
---

## 使用v-horizontal-scroll

> 网上一个大佬写的，是真的厉害
> 
> [el-table横向滚动条吸底处理方案思路 - 哔哩哔哩](https://www.bilibili.com/read/cv15185833)
> 
> 源码仓库地址
>
> [GitHub - mizuka-wu/el-table-horizontal-scroll: el-table awlays show horizontal-scroller on bottom](https://github.com/mizuka-wu/el-table-horizontal-scroll)

## 如何使用
### 安装
```
  npm install el-table-horizontal-scroll
```

### 注册指令
```vue
import horizontalScroll from 'el-table-horizontal-scroll'
Vue.use(horizontalScroll)
```

### 或者
```vue
import horizontalScroll from 'el-table-horizontal-scroll'
 
export default {
    directives: {
        horizontalScroll
    }
}
```

### 在el-table上添加v-horizontal-scroll
可以使用 always 或 hover ,默认值为hover，将鼠标悬停在表格上时，该栏将显示；

或者可以将其更改为always，并使栏始终显示

```vue
<el-table :data="data"  v-horizontal-scroll="'always'">
 
</el-table>
```
效果图（我自己就是选的这个方法，个人觉得比其他方法好）

![el-table 横向滚动条固定在可视窗口底部.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-f8b82e86-427b-4edb-96f2-96bfd1f37443/8a218d1b-7bea-4394-b31a-9d989c9ec187.png "el-table 横向滚动条固定在可视窗口底部")
