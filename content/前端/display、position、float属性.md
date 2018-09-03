---
title: display、position、float属性
layout: page
date: 2018-05-18 00:00
---
[TOC]
# display

- block
> 将元素显示为块级元素，元素前后会带有换行符独占一行，可自定义高宽。

- inline
> 将元素设为内联元素，元素前后没有换行符，高宽内外边距等定义无效。

- inline-block
> 将元素显示为行内块级元素，没有换行符，但可自定义高宽内外边距等。

- none
> 元素不会被现实，同时不占用文档流位置。

# position

- inhert
> 继承父元素的position值。

- static
> **默认值**，没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）

- relative
> 相对定位，相对元素本身进行正常定位，`eg：left: 20px`,会向元素的left位置移动20个px

- absolute
> 绝对定位，相对于 **static 定位以外的第一个祖先元素进行定位**。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。

- fixed
> 固定定位，**相对于浏览器窗口进行定位**。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。

# float
> 浮动值，不占用文档流位置


