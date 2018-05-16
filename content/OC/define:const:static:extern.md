---
title: define、const、static、extern
layout: page
date: 2018-05-14 00:00
---

- define: 通常所说的宏，宏是在预编译的时候处理**变量的替换**，宏除了定义变量还可以**定义一些方法**，宏不做编译检查，不报编译错误，大量使用宏会导致二进制文件过大，编译时间过长。

```
#define Identifie @"identifier"
```

- const: **修饰变量或指针变量只读不能修改**

```
//利用const定义一个全局只读变量 
NSString  * const Kname = @"key";
```

- static: **将变量的访问范围控制在一个文件内**

    - **修饰局部变量：**保证局部变量只初始化一次。

```
    for (int k=0; k<2; k++) {
       static int i = 1;
        i++;
        NSLog(@"i的值=%d",i);
    }
    输出：
    i的值=2
    i的值=3
```
    
    - **修饰全局变量：**使全局变量的作用域仅限于当前文件内，比较典型的是使用GCD一次性函数创建的单例

    
```
    //static修饰全局变量，让外界文件无法访问
    static LoginTool *_sharedManager = nil;
    + (LoginTool *)sharedManager {   
       static dispatch_once_t oncePredicate;   
       dispatch_once(&oncePredicate, ^{
            _sharedManager = [[self alloc] init];
        });   
       return _sharedManager;
    }
```
    
    
- extern: **将变量访问范围扩展**，我们可以在.h文件中extern声明一些全局的常量，在.m文件中实现或赋值，这样，只要导入头文件，就可以全局的使用定义的变量或常量。

```
//声明一些全局常量
extern NSString * const name;
extern NSInteger const count;
然后在.m文件中去实现
#import //实现
NSString * const name = @"王五";
NSInteger const count = 3;
``` 

- **static与const联合使用，声明一个文件中的只读常量**

```
// 开发中经常拿到key修改值，因此用const修饰key,表示key只读，不允许修改。
static  NSString * const key = @"name";
// 如果 const修饰 *key1,表示*key1只读，key1还是能改变。
static  NSString const *key1 = @"name";
```

- **extern与const联合使用定义一份全局常量，多个文件共享。**

```
•    GlobalConst.h
extern NSString * const nameKey = @"name";

•   GlobalConst.m
#import <Foundation/Foundation.h>

NSString * const nameKey = @"name"
```


