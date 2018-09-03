---
title: RN通信机制
layout: page
date: 2018-04-02 00:00
--- 


- js调用OC代码

> js调用OC函数 => 通过配置表将函数转化为ModuleID/MethondID然后传递给 => OC根据ID通过配置表找到对应的方法 => 处理参数（参数类型转换）=> 调用OC方法 => 回调CallBackID给JS => JS通过CallbackID拿到callback执行



![](https://ws2.sinaimg.cn/large/0069RVTdgy1fu0zuge17aj30o80hv76d.jpg)

- 参考资料

>[React Native通信机制详解](http://blog.cnbang.net/tech/2698/)
[ReactNative iOS源码解析（一）](http://awhisper.github.io/2016/06/24/
ReactNative%E6%B5%81%E7%A8%8B%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/)


