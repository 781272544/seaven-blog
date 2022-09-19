---
title: flex布局各种对齐方式
date: 2022-09-19 08:49:56
tags: ["CSS","CSS3","flex"]
categories: ["CSS3"]
---

```scss
/* flex布局 */
	.flex-row {
		display: flex;
		flex-direction: row;
	}
 
	/* 水平平均分布 */
	.flex_around {
		@extend .flex-row;
		justify-content: space-around;
	}
 
	/* 水平两端对齐 */
	.flex_between {
		@extend .flex-row;
		justify-content: space-between;
	}
 
	/** 水平右对齐 */
	.flex_end {
		@extend .flex-row;
		justify-content: flex-end;
	}
 
	/* 水平居中对齐 */
	.flex_center {
		@extend .flex-row;
		justify-content: center;
	}
 
	/* 垂直居中 */
	.flex_center_align {
		@extend .flex-row;
		align-items: center;
	}
 
	/* 水平平均垂直居中 */
	.flex_center_around {
		@extend .flex-row;
		justify-content: space-around;
		align-items: center;
	}
```
