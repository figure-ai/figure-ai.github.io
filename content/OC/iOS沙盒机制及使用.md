---
title: iOS沙盒机制及使用
layout: page
date: 2018-04-02 00:00
---

[TOC]

## 前言
> iOS系统的沙盒机制规定每个应用**只能访问当前沙盒目录下的文件**，不可跨应用访问数据。

##沙盒目录结构

- 每个APP的沙盒目录结构如下图


![](https://ws3.sinaimg.cn/large/005P98D1gy1fqq30g0lxhj30lk0ne448.jpg)


###AppName.app目录
>应用程序包目录，包含应用程序本身，由于应用程序必须经过签名，所以在运行时不能对这个目录中的内容进行修改，否则可能会使应用程序无法启动。

###Documents目录
> 用于**存储用户数据**，除非APP卸载,不然里面东西是不会丢失的,但是也不能存放大文件和下载的东西.在手机连接iTunes备份和iCould备份时会备份此文件夹里面的东西

###Library目录
> 这个目录下有多个子目录，除**Caches目录外**其他目录都会被iTunes和iCloud备份

####Library/Perferences目录
>包含应用程序的**偏好设置文件**，您不应该直接创建偏好设置文件，而是应用**使用NSUserDefaults类**来取得和**设置应用程序的偏好设置**

#### Library/Application Support目录
>使用此目录存储除与用户文档相关的所有应用程序数据文件。例如，您可以使用此目录存储由应用程序管理/创建的数据文件，配置文件.


#### Library/Caches目录
>1. 用于存放应用程序专用的支持文件，保存应用程序再次启动过程中需要的信息。
>2. 可创建子文件夹，可以用来设置您希望备份但不希望被用户看到的数据，该路径下的文件夹
>3. 通常用于存放缓存文件，比如SDWebImage的缓存数据就放在这个目录下。

###tmp目录
>存放临时文件，在App关闭再启动就没有了，不能放重要的文件
>**此目录的内容不会由iTunes或iCloud备份。**


## 获取各种文件目录的路径

- 获取目录路径的方法

```
// 获取沙盒主目录路径
NSString *homeDir = NSHomeDirectory();
// 获取Documents目录路径
NSString *docDir = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];
//
// 获取Library的目录路径
NSString *libDir = [NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES) lastObject];
// 获取Caches目录路径
NSString *cachesDir = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];
// 获取tmp目录路径
NSString *tmpDir =  NSTemporaryDirectory();

```

- 获取程序包中资源文件夹路径的方法

```
NSLog(@"%@",[[NSBundle mainBundle] bundlePath]);
NSString *imagePath = [[NSBundle mainBundle] pathForResource:@"apple" ofType:@"png"];
UIImage *appleImage = [[UIImage alloc] initWithContentsOfFile:imagePath];

```

## NSSearchPathForDirectoriesInDomains
> NSSearchPathForDirectoriesInDomains方法用于查找目录，返回指定范围内的指定名称的目录的路径集合。有三个参数

- directory:

    > NSSearchPathDirectory类型的enum值，表明我们要搜索的目录名称，比如这里用NSDocumentDirectory表明我们要搜索的是Documents目录。如果我们将其换成NSCachesDirectory就表示我们搜索的是Library/Caches目录。


- domainMask:

    > NSSearchPathDomainMask类型的enum值，指定搜索范围，这里的NSUserDomainMask表示搜索的范围限制于当前应用的沙盒目录。还可以写成NSLocalDomainMask（表示/Library）、NSNetworkDomainMask（表示/Network）等。

- expandTilde:

    > BOOL值，表示是否展开波浪线。我们知道在iOS中的全写形式是/User/userName，该值为YES即表示写成全写形式，为NO就表示直接写成“~”。
该值为NO:Caches目录路径~/Library/Caches
该值为YES:Caches目录路径
/var/mobile/Containers/Data/Application/E7B438D4-0AB3-49D0-9C2C-B84AF67C752B/Library/Caches




