---
title: OC内存管理机制
layout: page
date: 2018-04-02 00:00
---

[TOC]

## 早期手动控制引用计数（MRC）
- Object-C继承自C，拥有一套基于**对象引用计数**的内存管理体系（当对象引用计数为0时会被自动释放）。
- 需要注意的是：**这套体系是对OC类型的内存管理，它对基本数据类型和C语言类型并不管用**

```
int,short,char,struct,enum,union等类型
OC类型：任何继承于NSObject对象都属于OC的类型。
```

- 早期的内存管理开发人员需要使用**alloc**(alloc会使得对象的引用计数初始化为1)、**retain**、**release**、控制对象的引用计数

```
//申请内存，并调用初始化，返回指针
//alloc会使得对象的引用计数初始化为1
Engine *engine1 = [[Engine alloc] init]; 

//调用retain，等同于使引用计数+1
[engine1 retain];

//调用release，等同于使引用计数-1
//只有当引用计数为0时，对象才会释放
[engine1 release];
```

- 假设有一个类，其中包含有多个属性，这些属性都是**指针类型**，这个时候就需要使用对象的**set**和**get**方法对了的字段进行封装

```
-(void) setName:(NSString *)name{
    if(self.Name != name){
        // 如果self.Name 不等于传进来的name，则释放旧的Name
        [Name release];
        Name = [name retain];
    }
}

-(NSString *) getName {
    return Name;
}

-(void)dealloc{
    [self setName:nil];
    [super dealloc];
}
```

## 自动释放池的出现(Autorelease pool)

- **Autorelease实际上只是把对release的调用延迟了，对于每一个Autorelease，系统只是把该Object放入了当前的Autorelease pool中，当该pool被释放时，pool中的所有Object会调用Release**
- 自动释放池的出现主要解决一个问题， **需要在一个方法内部申请内存，并将内存地址返回给调用者时，谁来释放这个内存**
- 对于每一个Runloop，系统会隐式创建一个Autorelease pool，这样所有的release pool会构成一个像CallStack的栈式结构，在每一个Runloop结束时，当前栈顶的Autorelease pool会被销毁，这样pool里的每一个Object会被release
- 自动释放池遵循的原则是：**谁申请谁释放**

```
The basic rule to apple is everything thatincreases the reference counter with alloc,[mutable]copy[WithZone:] or retainis in charge of the corresponding [auto]release.

如果一个对象使用了alloc，[mutable] copy，retain，那么必须使用相应的release或autorelease
```

## 自动引用计数（ARC）

- **自动引用计数（ARC）是编译器的一个功能**，也是目前开发所使用的，实际上**ARC只是编译器帮开发者完成了对引用计数的增减，并没有少了引用计数的环节。**


##OC对象在内存中的结构

- 实际上我们所说的OC内存管理是对OC类型的内存管理，它对基本数据类型和C语言类型是不管用的。

```
    int,short,char,struct,enum,union等类型
    
    OC类型：任何继承于NSObject对象都属于OC的类型。
```

- 所有OC类型的对象（任何继承于NSObject对象的的类型）都会包含一个retainCount的计数器属性

- 计算规则
    1. Objective-C类中实现了引用计数器，对象知道自己当前被引用的次数
    2. 初始化时对象的计数器为1
    3. 如果需要引用对象，可以给对象发送一个retain消息，这样对象的计数器就加1
    4. 当不需要引用对象了，可以给对象发送release消息，这样对象计数器就减1
    5. 当计数器减到0，自动调用对象的dealloc函数，对象就会释放内存
    6. 计数器为0的对象不能再使用release和其他方法

- property属性对应的setter写法
    
    1. **assign**：告诉编译器**不要retain**传入的参数（一般用于基本数据类型）
        
    ```
    -(void)setEngine:(Engine*) engine  
    {  
         _engine=engine;  
    }  
    ```
    
    2. **retain**:告诉编译器在把setter参数传给变量前，**要先retain一下**（通常用于对象属性）
        
    ```
    -(void)setEngine:(Engine*) engine  
    {  
       if(_engine!=engine){//判断是否重复设置  
               [_engine release];//在设置之前，先release，那么在设置的时候，就会自动将前面的一个引用release掉  
               _engine=[engine retain];//多了一个引用，retainCount+1  
          }  
     }  
    ```
        
    3. **copy**: 在setter参数赋值给实例变量之前，**先copy一下**。（通常用于字符串属性）

    ```
    -(void)setEngine:(Engine*) engine  
    {  
       if(_engine!=engine){//判断是否重复设置  
               [_engine release];//在设置之前，先release，那么在设置的时候，就会自动将前面的一个引用release掉  
               _engine=[engine copy];//多了一个引用，retainCount+1  
          }  
     }  
    ```
    
##对象引用计数的缺点
> **无法检测出循环引用**。如父对象有一个对子对象的引用，子对象反过来引用父对象。这样，他们的引用计数永远不可能为0。

## 相关资料

>- [Objective-C 内存管理的历史和参考资料](https://segmentfault.com/a/1190000002879323)
- [iOS:内存管理（一）：OC中的内存管理](http://www.cnblogs.com/mybkn/articles/3123967.html)
- [iOS:内存管理（二）：怎样在xcode里面使用Memory Leaks和Instruments教程](http://www.cnblogs.com/mybkn/articles/3123981.html)
- [iOS:内存管理（三）：在Objective-c里面使用property教程](http://www.cnblogs.com/mybkn/articles/3124000.html)
- [Objective-C的内存管理（一）黄金法则的理解](https://blog.csdn.net/lonelyroamer/article/details/7666851)
- [Objective-C的内存管理（二）autorelease的理解](https://blog.csdn.net/lonelyroamer/article/details/7673940)


