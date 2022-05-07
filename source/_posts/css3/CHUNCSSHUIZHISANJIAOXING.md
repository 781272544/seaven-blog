---
title: 纯CSS绘制三角形（各种角度）
date: 2022-05-07 16:32:26
tags: ["CSS","CSS3","三角形"]
categories: ["CSS3"]
---

> CSS三角形绘制方法，学会了这个，其它的也就简单。

我们的网页因为 CSS 而呈现千变万化的风格。这一看似简单的样式语言在使用中非常灵活，只要你发挥创意就能实现很多比人想象不到的效果。特别是随着 CSS3 的广泛使用，更多新奇的 CSS 作品涌现出来。

今天给大家带来 CSS 三角形绘制方法

![triangleUp.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/98d3c026-ba85-4093-b761-06587e008198.jpg "triangleUp")

```css
#triangle-up {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-bottom: 100px solid red;
}
```

![triangleDown.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/6d6ef951-9bed-4a2e-87a1-073f374b4c31.jpg "triangleDown")

```css
#triangle-down {
    width: 0;
    height: 0;
    border-left: 50px solid transparent;
    border-right: 50px solid transparent;
    border-top: 100px solid red;
}
```

![triangleLeft.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/a0a5affc-a05b-4015-8ae9-03b5d331f856.jpg "triangleLeft")

```css
#triangle-left {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-right: 100px solid red;
    border-bottom: 50px solid transparent;
}
```

![triangleRight.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/29ef2288-f7b4-479c-8eb5-77dc1f842459.jpg "triangleRight")

```css
#triangle-right {
    width: 0;
    height: 0;
    border-top: 50px solid transparent;
    border-left: 100px solid red;
    border-bottom: 50px solid transparent;
}
```

![triangleTopLeft.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/1f824ea2-a882-4d69-827a-693f7f94beba.jpg "triangleTopLeft")

```css
#triangle-topleft {
    width: 0;
    height: 0;
    border-top: 100px solid red;
    border-right: 100px solid transparent;
}
```

![triangleTopRight.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/69311735-a1af-49ce-9ca9-e5f2c062421e.jpg "triangleTopRight")

```css
#triangle-topright {
    width: 0;
    height: 0;
    border-top: 100px solid red;
    border-left: 100px solid transparent;
}
```

![triangleBottomLeft.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/c0e60bf2-f538-4572-9bc6-2aa32fcd543b.jpg "triangleBottomLeft")

```css
#triangle-bottomleft {
    width: 0;
    height: 0;
    border-bottom: 100px solid red;
    border-right: 100px solid transparent;
}
```

![triangleBottomRight.png](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-b239efaa-5152-4c7c-a688-7f7519bc8433/8fa12a1a-126f-4c69-8964-9144d140af9b.jpg "triangleBottomRight")

```css
#triangle-bottomright {
    width: 0;
    height: 0;
    border-bottom: 100px solid red;
    border-left: 100px solid transparent;
}
```


