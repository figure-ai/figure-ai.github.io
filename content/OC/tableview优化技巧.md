---
title: tableview优化技巧
layout: page
date: 2018-05-14 00:00
---


- 预排版

```
在后台计算好布局属性，减少tableview在请求各个cell的高度时的计算量
```

- 预渲染

```
在后台线程将头像预先渲染为圆形并单独保存到一个 ImageCache 中去。
尽量避免使用 layer 的 border、corner、shadow、mask 等技术，而尽量在后台线程预先绘制好对应内容。
```


- 异步绘制


