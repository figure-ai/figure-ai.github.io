---
title: Autorelease pool
layout: page
date: 2018-04-02 00:00
---

[TOC]

##相关知识
1. autorelease pool什么时候创建
    
    > 在NSRunLoop对象的每个**运行循环（event loop）**开始前，系统会自动创建一个autoreleasepool

2. autorelease pool 什么时候释放

    > 在**没有手动添加Autorelease Pool** 的情况下，autorelease pool 会在**当前runloop迭代结束时**释放。
    
3. autorelease pool 做了什么

    > Autorelease Pool释放时，添加进pool的对象也会随之释放
    
4. NSThread、NSRunLoop、NSAutoreleasePool三者之间的关系
    
    1. NSThread和NSRunLoop是**一一对应的关系**
    2. 在NSRunLoop每个**运行循环开始前**，系统会自动创建一个autoreleasepool，并在**运行循环结束时drain掉这个pool**同时是释放掉所有autoreleased对象
    3. autorelease pool只会对应个线程，每个线程可以对应多个autorelease pool，比如autorelease pool嵌套的情况

5. 需要**手动添加**autoreleasepool的情况
    
    1. 如果你编写的程序**不是基于UI框架的**，比如命令行工具
    2. for循环遍历中产生**大量autorelease变量时**
    3. **创建了一个子线程**，一般会自定义**继承自NSOpreation的操作**，这段代码是在子线程上执行的无法访问主线程的自动释放池，所以得自己创建。在main方法中要加上**@autoreleasepool{...}**。
    4. **长时间在后台运行的任务**

##其他知识

1. 使用容器的block的枚举器时，内部会自动添加一个autoreleasepool

```
[array enumerateObjectsUsingBlock:^(id obj, NSUInteger ind, BOOL *stop) {
    // 这里会被一个局部@autoreleasepool{}包裹着
}];
```
2. 普通的**for循环**和**for-in循环**中没有，但是性能还是**for-in**最高




