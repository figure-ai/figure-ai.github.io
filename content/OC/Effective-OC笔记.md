---
title: Effective-OC笔记
layout: page
date: 2018-05-14 00:00
---

- 尽量减少引入头文件，多使用@class向前声明类

```
比如在person.h文件中导入book.h，那么所有导入person.h的文件默认导入book.h不管有没有用到，这势必会增加编译时长。
```
- 多用字面量语法，字面量语法创建的**为不可变对象**

```
// 字面量语法
NSDictionary *dict = @{@"hight": @(200), @"name": @"lyly"}
```
- 多用类型常量，少用#define预处理命令

```
// 类型常量，staic声明变量只在此文件可见，const声明变量不可修改
staic const NSString *kCellId = @"cellID"

// #define预处理命令，使用预处理命令会将代码中的CELL_ID替换成cellID，但是没有类型信息。
// 且适用于所有文件
#define CELL_ID cellID
```

- 用枚举表示状态、选项、状态码

```
enum EOCConnectState {
    EOCEnumConnect,
    EOCEnumDisconnect,
};
type of enum EOCConnectState = EOCConnectState;

// 向前声明枚举类型
// 这样写的好处是在用到变量时，编译器知道变量是什么类型，从而清楚要分配多少内存空间
enum EOCConectState: NSInteger {
    EOCEnumConnect,
    EOCEnumDisconnect,
}
type of enum EOCConnectState = EOCConnectState;

// 向前声明枚举类型写法2
// 这样写会把EOCEnumConnect的值置为1，后面的状态会依次递增。
// 比如EOCEnumConnected的值就为3
enum EOCConectState {
    EOCEnumConnect = 1,
    EOCEnumDisconnect,
    EOCEnumConnected,
}
type of enum EOCConnectState = EOCConnectState;

// 定义选项时使用“按位或操作符”组合枚举， 这样定义的好处是外部使用的时候可以选择多个选项
enum UIViewAutoresizing {
    UIViewAutoresizingnone                  = 0,
    UIViewAutoresizingFlexibleLeftMargin    = 1 << 0,
    UIViewAutoresizingFlexibleRightMargin   = 1 << 1,
    UIViewAutoresizingFlexibleTopMargin     = 1 << 2,
    UIViewAutoresizingFlexibleBottomMargin  = 1 << 3,
    UIViewAutoresizingFlexibleHeight        = 1 << 4,
    UIViewAutoresizingFlexibleWidth         = 1 << 5,
}

// NS_ENUM和NS_OPTIONS宏都是使用#define预处理指令来定义的定义
// 使用NSEnum宏定义一个枚举
typedef NS_ENUM(NSUInteger, EOCConnectionState) {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected
}
// NSOption
typedef NS_OPTIONS(NSUInteger, EOCConnectionState) {
    EOCConnectionStateDisconnected  = 1 << 0,
    EOCConnectionStateConnecting    = 1 << 1,
    EOCConnectionStateConnected     = 1 << 2
}


```




