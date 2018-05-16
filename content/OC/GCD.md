---
title: GCD的使用
layout: page
date: 2018-04-02 00:00
---


[TOC]

## GCD简介
> GCD全称**Grand Central Dispatch**，是苹果公司为**多核并行运算**提出的解决方案，可以优化应用以支持多核处理器以及其他对称多处理系统。


## GCD的优势
 - 自动利用更多的CPU内核（如：双核、四核）
 - 自动管理线程的生命周期（创建线程、调度任务、销毁线程等，ARC模式下）
 - 自动管理线程的生命周期（创建线程、调度任务、销毁线程）
 - 程序员只需要告诉GCD想要执行什么任务，不需要编写任何线程管理代码。

## GCD任务和队列

### 任务
> 任务就是线程中要执行的代码，在GCD中是放在block中的，执行任务的方式有两种：**同步执行（sync）和异步执行（async）**

1. **同步执行（sync）**

	 - 不具备开启新线程的能力
	 - 在添加的任务执行结束之前，会一直等待，直到任务完成再执行下一个任务。
2. **异步执行（async）**

	 - 具备开启新线程的能力，但不一定都会开启新线程，更任务所指定的队列类型有关。
	 - 无需等待任务返回，可以继续执行下一个任务，同时执行多个任务。
### 队列
> 用来**存放任务**的等待队列，是一种特殊的线性表，采用FIFO（先进先出）的原则。GCD中有两种队列：**串行队列和并行队列**

1. **串行队列**

	- 只开启一个线程，按顺序执行，每次只有一个任务被执行，一个任务执行完毕后，再执行下一个任务。

2. **并发队列**
	 - 可以开启多个线程，并且同时执行多个任务
	 - 并发队列的并发功能只有在异步函数下才有效
## GCD的使用步骤

1. 创建一个队列
2. 创建任务，并将任务追加到队列中，然后系统就会根据任务类型执行任务。

### 队列的创建/获取方法

> 队列可以使用：`dispatch_queu_create()`来创建，这个函数传入两个参数：
> 1. 第一个参数表示队列的唯一标识符，用于DEBUG，可为空，（推荐使用应用程序ID这种逆序全程域名）；
> 2. 第二个参数用来识别是串行（`DISPATCH_QUEUE_SERIAL`）队列还是并行队列（`DISPATCH_QUEUE_CONCURRENT`）

1. 创建串行队列

```
dispatch_queue_t serialQueue = dispatch_queue_create("com.test.ch", DISPATCH_QUEUE_SERIAL);
```
	
2. 创建并行队列

```
dispatch_queue_t crnQueue = dispatch_queue_create("com.test.ch",DISPATCH_QUEUE_CONCURRENT);
```
	
3. 获取主队列
	> 主队列是GCD提供的一种串行队列，所有放在主队列中的任务，都会放到**主线程中执行**
	
```
// 获取主队列
dispatch_queue_t mainQueue = dispatch_get_main_queue();
```
	
4. 获取全局并发队列（Global Dispatch Queue）
	> 全局并发队列是GCD提供的一种并发队列。
	> 1. 可以使用`dispatch_get_global_queue`来获取，需要提供两个参数，第一个设置队列优先级，一般用`DISPATSH_QUEUE_PRIORITY_DEFAULT`，第二个参数暂时没用，传0即可。
	
```
// 获取全局并发队列
dispatch_queue_t globalQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT,0);
```

### 任务的创建方法

1. 创建同步执行任务

``` 
	// 创建同步执行任务，queue为指定存放任务的队列
	dispatch_sync(queue, ^{
	    // 这里放同步执行任务代码
	}); 
```

2. 创建异步执行任务

```
// 创建异步执行任务，queue为指定存放任务的队列
dispatch_async(queue,^{
    // 这里放异步执行任务代码
});
```

## GCD的基本使用
> 根据队列类别和任务执行方式GCD的使用有以下几种组合方式。

### 同步执行+并行队列

> 在当前线程中执行任务，**不会开启**子线程，任务**有序**执行

```
// 同步并行
- (void)syncConcurrent {
    NSLog(@"currentThread---%@",[NSThread currentThread]);  // 打印当前线程
    NSLog(@"syncConcurrent---begin");
    //创建一个并行队列
    dispatch_queue_t concurrentQueue = dispatch_queue_create("sysnc_concurrent_test", DISPATCH_QUEUE_CONCURRENT);
    
    // 添加同步执行任务1
    dispatch_sync(concurrentQueue, ^{
        //
        [NSThread sleepForTimeInterval:3];    // 模拟耗时操作
        NSLog(@"1---%@",[NSThread currentThread]);      // 打印当前线程
    });
    
    // 添加同步执行任务2
    dispatch_sync(concurrentQueue, ^{
        //
        [NSThread sleepForTimeInterval:2];    // 模拟耗时操作
        NSLog(@"2---%@",[NSThread currentThread]);      // 打印当前线程
    });
    
    // 添加同步执行任务2
    dispatch_sync(concurrentQueue, ^{
        //
        [NSThread sleepForTimeInterval:1];    // 模拟耗时操作
        NSLog(@"3---%@",[NSThread currentThread]);      // 打印当前线程
    });
    
    NSLog(@"syncConcurrent--------end");
}
```

> 打印结果
> 2018-04-19 17:44:08.849921+0800 GCD\_Test[8535:933000] currentThread---\<NSThread: 0x600000260d00\>{number = 1, name = main}
2018-04-19 17:44:08.850056+0800 GCD\_Test[8535:933000] syncConcurrent---begin
2018-04-19 17:44:11.851112+0800 GCD\_Test[8535:933000] 1---\<NSThread: 0x600000260d00\>{number = 1, name = main}
2018-04-19 17:44:13.852772+0800 GCD\_Test[8535:933000] 2---\<NSThread: 0x600000260d00\>{number = 1, name = main}
2018-04-19 17:44:14.854313+0800 GCD\_Test[8535:933000] 3---\<NSThread: 0x600000260d00\>{number = 1, name = main}
2018-04-19 17:44:14.854720+0800 GCD\_Test[8535:933000] syncConcurrent--------end

分析：
	1. 因为同步执行不具备开启新线程的能力，所以所有任务都是在**当前线程（主线程）**中执行的。
	2. 因为同步执行需要等上一个任务执行完毕才会执行下一个任务，所以任务会按序执行。

### 异步执行+并发队列
> 会开启多个线程，任务同时执行

```
// 异步并行
- (void)asyncConcurrent {
    NSLog(@"currentThread---%@",[NSThread currentThread]);  // 打印当前线程
    NSLog(@"asyncConcurrent---begin");
    //创建一个并行队列
    dispatch_queue_t concurrentQueue = dispatch_queue_create("sysnc_concurrent_test", DISPATCH_QUEUE_CONCURRENT);
    //添加异步执行任务1
    dispatch_async(concurrentQueue, ^{
        [NSThread sleepForTimeInterval:3];    // 耗时3秒
        NSLog(@"任务1-耗时3秒--%@",[NSThread currentThread]);      // 打印当前线程
    });
    
    //添加异步执行任务2
    dispatch_async(concurrentQueue, ^{
        [NSThread sleepForTimeInterval:1];    // 耗时1秒
        NSLog(@"任务2-耗时1秒--%@",[NSThread currentThread]);      // 打印当前线程
    });
    
    //添加异步执行任务3
    dispatch_async(concurrentQueue, ^{
        [NSThread sleepForTimeInterval:2];    // 耗时2秒
        NSLog(@"任务3-耗时2秒--%@",[NSThread currentThread]);      // 打印当前线程
    });
    
    NSLog(@"syncConcurrent--------end");
}

```

> 打印结果：
> 2018-04-19 17:56:04.424675+0800 GCD\_Test[8634:955301] currentThread---\<NSThread: 0x600000261a00\>{number = 1, name = main}
2018-04-19 17:56:04.424812+0800 GCD\_Test[8634:955301] asyncConcurrent---begin
2018-04-19 17:56:04.424926+0800 GCD\_Test[8634:955301] syncConcurrent--------end
2018-04-19 17:56:05.425785+0800 GCD\_Test[8634:955443] 任务2-耗时1秒--\<NSThread: 0x600000471dc0\>{number = 4, name = (null)}
2018-04-19 17:56:06.426400+0800 GCD\_Test[8634:955439] 任务3-耗时2秒--\<NSThread: 0x604000668900\>{number = 5, name = (null)}
2018-04-19 17:56:07.427697+0800 GCD\_Test[8634:955440] 任务1-耗时3秒--\<NSThread: 0x6040002619c0\>{number = 6, name = (null)}

### 同步执行+串行队列

> 不会开启新线程，只在当前线程执行任务，**按序**执行

```
// 同步串行
- (void)syncSerial {
    NSLog(@"currentThread---%@",[NSThread currentThread]);  // 打印当前线程
    NSLog(@"syncSerial---begin");
    // 创建一个串行队列
    dispatch_queue_t serialQueue = dispatch_queue_create("com.lch.test.GCD-Test", DISPATCH_QUEUE_SERIAL);
    // 添加同步任务1
    dispatch_sync(serialQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务1--耗时1秒--%@",[NSThread currentThread]);
    });
    // 添加同步任务2
    dispatch_sync(serialQueue, ^{
        [NSThread sleepForTimeInterval:3];
        NSLog(@"任务2--耗时3秒--%@",[NSThread currentThread]);
    });
    // 添加同步任务3
    dispatch_sync(serialQueue, ^{
        [NSThread sleepForTimeInterval:2];
        NSLog(@"任务3--耗时2秒--%@",[NSThread currentThread]);
    });
    NSLog(@"syncSerial---end");
}
```

> 打印结果：
> 2018-04-19 18:05:59.608220+0800 GCD\_Test[8736:977132] currentThread---\<NSThread: 0x60400007e980\>{number = 1, name = main}
2018-04-19 18:05:59.608341+0800 GCD\_Test[8736:977132] syncSerial---begin
2018-04-19 18:06:00.608839+0800 GCD\_Test[8736:977132] 任务1--耗时1秒--\<NSThread: 0x60400007e980\>{number = 1, name = main}
2018-04-19 18:06:03.609289+0800 GCD\_Test[8736:977132] 任务2--耗时3秒--\<NSThread: 0x60400007e980\>{number = 1, name = main}
2018-04-19 18:06:05.610914+0800 GCD\_Test[8736:977132] 任务3--耗时2秒--\<NSThread: 0x60400007e980\>{number = 1, name = main}
2018-04-19 18:06:05.611239+0800 GCD\_Test[8736:977132] syncSerial---end

### 异步执行+串行

> 会**开启一个新线程**，但是因为任务是串行的，所以会按序执行

```
// 异步串行
- (void)asyncSerial {
    NSLog(@"currentThread---%@",[NSThread currentThread]);  // 打印当前线程
    NSLog(@"asyncSerial---begin");
    //创建一个串行队列
    dispatch_queue_t serialQueue = dispatch_queue_create("com.lch.test.GCD-Test", DISPATCH_QUEUE_SERIAL);
    // 添加异步任务1
    dispatch_async(serialQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务1--耗时1秒--%@",[NSThread currentThread]);
    });
    // 添加异步任务2
    dispatch_async(serialQueue, ^{
        [NSThread sleepForTimeInterval:3];
        NSLog(@"任务2--耗时3秒--%@",[NSThread currentThread]);
    });
    // 添加异步任务3
    dispatch_async(serialQueue, ^{
        [NSThread sleepForTimeInterval:2];
        NSLog(@"任务3--耗时2秒--%@",[NSThread currentThread]);
    });
    NSLog(@"asyncSerial---end");
}
```

> 打印结果：
> 2018-04-19 18:12:03.514190+0800 GCD\_Test[8788:990727] currentThread---\<NSThread: 0x60000007a9c0\>{number = 1, name = main}
2018-04-19 18:12:03.514312+0800 GCD\_Test[8788:990727] asyncSerial---begin
2018-04-19 18:12:03.514409+0800 GCD\_Test[8788:990727] asyncSerial---end
2018-04-19 18:12:04.515561+0800 GCD\_Test[8788:991002] 任务1--耗时1秒--\<NSThread: 0x600000667a40\>{number = 5, name = (null)}
2018-04-19 18:12:07.516071+0800 GCD\_Test[8788:991002] 任务2--耗时3秒--\<NSThread: 0x600000667a40\>{number = 5, name = (null)}
2018-04-19 18:12:09.521700+0800 GCD\_Test[8788:991002] 任务3--耗时2秒--\<NSThread: 0x600000667a40\>{number = 5, name = (null)}

### 同步执行+主队列

#### 主线程中调用
> 互相等待，主线程卡死

```
// 主队列同步执行
- (void)syncMainQueue {
    NSLog(@"currentThread---%@",[NSThread currentThread]);  // 打印当前线程
    NSLog(@"syncMainQueue---begin");
    // 获取主队列
    dispatch_queue_t mainQueue = dispatch_get_main_queue();
    //添加同步任务1
    dispatch_sync(mainQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务1--耗时1秒--%@",[NSThread currentThread]);
    });
    
    // 添加同步任务2
    dispatch_sync(mainQueue, ^{
        [NSThread sleepForTimeInterval:3];
        NSLog(@"任务2--耗时2秒--%@",[NSThread currentThread]);
    });
    // 添加同步任务3
    dispatch_sync(mainQueue, ^{
        [NSThread sleepForTimeInterval:2];
        NSLog(@"任务3--耗时3秒--%@",[NSThread currentThread]);
    });
    NSLog(@"syncMainQueue---begin");
}

```

#### 其他线程调用
> 不会开启新线程，按序执行

```
// 使用 NSThread 的 detachNewThreadSelector 方法会创建线程，并自动启动线程执行
    [NSThread detachNewThreadSelector:@selector(syncMainQueue) toTarget:self withObject:nil];

```

> 打印结果
> 2018-04-19 18:49:13.696441+0800 GCD\_Test[9146:1054660] currentThread---\<NSThread: 0x604000466c40\>{number = 5, name = (null)}
2018-04-19 18:49:13.696600+0800 GCD\_Test[9146:1054660] syncMainQueue---begin
2018-04-19 18:49:14.702150+0800 GCD\_Test[9146:1054248] 任务1--耗时1秒--\<NSThread: 0x604000079880\>{number = 1, name = main}
2018-04-19 18:49:17.705675+0800 GCD\_Test[9146:1054248] 任务2--耗时2秒--\<NSThread: 0x604000079880\>{number = 1, name = main}
2018-04-19 18:49:19.727515+0800 GCD\_Test[9146:1054248] 任务3--耗时3秒--\<NSThread: 0x604000079880\>{number = 1, name = main}
2018-04-19 18:49:19.727875+0800 GCD\_Test[9146:1054660] syncMainQueue---begin

### 异步执行+主队列
> 只在主线程中执行任务，执行完一个再执行下一个

```
// 主队列异步执行
- (void)asyncMainQueue {
    NSLog(@"currentThread---%@",[NSThread currentThread]);  // 打印当前线程
    NSLog(@"asyncMainQueue---begin");
    // 获取主队列
    dispatch_queue_t mainQueue = dispatch_get_main_queue();
    dispatch_async(mainQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务1--耗时1秒--%@",[NSThread currentThread]);
    });
    dispatch_async(mainQueue, ^{
        [NSThread sleepForTimeInterval:3];
        NSLog(@"任务2--耗时3秒--%@",[NSThread currentThread]);
    });
    dispatch_async(mainQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务3--耗时2秒--%@",[NSThread currentThread]);
    });
    NSLog(@"syncMainQueue---begin");
}
```

> 打印结果
> 2018-04-19 18:57:09.221843+0800 GCD\_Test[9199:1069120] currentThread---\<NSThread: 0x604000067bc0\>{number = 1, name = main}
2018-04-19 18:57:09.221979+0800 GCD\_Test[9199:1069120] asyncMainQueue---begin
2018-04-19 18:57:09.222149+0800 GCD\_Test[9199:1069120] syncMainQueue---begin
2018-04-19 18:57:10.229029+0800 GCD\_Test[9199:1069120] 任务1--耗时1秒--\<NSThread: 0x604000067bc0\>{number = 1, name = main}
2018-04-19 18:57:13.230537+0800 GCD\_Test[9199:1069120] 任务2--耗时3秒--\<NSThread: 0x604000067bc0\>{number = 1, name = main}
2018-04-19 18:57:14.232182+0800 GCD\_Test[9199:1069120] 任务3--耗时2秒--\<NSThread: 0x604000067bc0\>{number = 1, name = main}

## GCD线程间的通信
> 使用场景：比如在子线程下载数据，然后到主线程刷新界面

```
- (void)queueCommunication {
    // 获取global队列
    dispatch_queue_t globalQueue = dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0);
    // 获取主队列
    dispatch_queue_t mainQueue = dispatch_get_main_queue();
    dispatch_async(globalQueue, ^{
        //处理耗时操作
        [NSThread sleepForTimeInterval:3];
        NSLog(@"--处理耗时操作---%@",[NSThread currentThread]);
        dispatch_async(mainQueue, ^{
            NSLog(@"--回到主线程--%@",[NSThread currentThread]);
        });
    });
}
```
> 打印结果：
> 2018-04-19 19:09:30.741262+0800 GCD\_Test[9314:1094783] --处理耗时操作---\<NSThread: 0x600000474240\>{number = 5, name = (null)}
2018-04-19 19:09:30.741861+0800 GCD\_Test[9314:1094724] --回到主线程--\<NSThread: 0x60000007c1c0\>{number = 1, name = main}

## GCD的其他方法

### ∑dispatch\_barrier\_async

> `dispatch_barrier_async()`方法：**先执行栅栏前的任务，然后在执行栅栏中的任务，再执行栅栏后的任务**


```
- (void)barrierAsync {
    // 创建一个并行队列
    dispatch_queue_t concurrent = dispatch_queue_create("com.lch.test.GCD-Test", DISPATCH_QUEUE_CONCURRENT);
    // 追加异步任务1
    dispatch_async(concurrent, ^{
       //
        [NSThread sleepForTimeInterval:3];
        NSLog(@"任务1---耗时3秒--%@",[NSThread currentThread]);
    });
    // 追加异步任务2
    dispatch_async(concurrent, ^{
        //
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务2---耗时1秒--%@",[NSThread currentThread]);
    });
    // 添加栅栏
    dispatch_barrier_sync(concurrent, ^{
        //
        [NSThread sleepForTimeInterval:2];
        NSLog(@"栅栏---耗时2秒--%@",[NSThread currentThread]);
    });
    // 追加异步任务3
    dispatch_async(concurrent, ^{
        //
        [NSThread sleepForTimeInterval:1];
        NSLog(@"任务3---耗时1秒--%@",[NSThread currentThread]);
    });
    // 添加栅栏
    dispatch_barrier_sync(concurrent, ^{
        //
        [NSThread sleepForTimeInterval:2];
        NSLog(@"栅栏---耗时2秒--%@",[NSThread currentThread]);
    });
    
    // 追加异步任务4
    dispatch_async(concurrent, ^{
        //
        [NSThread sleepForTimeInterval:2];
        NSLog(@"任务4---耗时2秒--%@",[NSThread currentThread]);
    });
}
```
> 打印结果：
> 2018-04-20 11:58:30.231174+0800 GCD\_Test[10572:1359087] 任务2---耗时1秒--\<NSThread: 0x600000279600\>{number = 3, name = (null)}
2018-04-20 11:58:32.232077+0800 GCD\_Test[10572:1359090] 任务1---耗时3秒--\<NSThread: 0x60000047bd80\>{number = 5, name = (null)}
2018-04-20 11:58:34.233081+0800 GCD\_Test[10572:1359006] 栅栏---耗时2秒--\<NSThread: 0x600000071340\>{number = 1, name = main}
2018-04-20 11:58:35.235600+0800 GCD\_Test[10572:1359090] 任务3---耗时1秒--\<NSThread: 0x60000047bd80\>{number = 5, name = (null)}
2018-04-20 11:58:37.237299+0800 GCD\_Test[10572:1359006] 栅栏---耗时2秒--\<NSThread: 0x600000071340\>{number = 1, name = main}
2018-04-20 11:58:39.237854+0800 GCD\_Test[10572:1359090] 任务4---耗时2秒--\<NSThread: 0x60000047bd80\>{number = 5, name = (null)}

### GCD延时执行方法：dispatch\_after
> `dispatch_after` :可以设定多长时间后执行任务
> 需要注意的是：函数并不是在指定时间后才开始执行，而是爱指定时间之后将任务追加到主队列，严格来说这个时间并不是绝对准确。

```
- (void)afterAsync {
    NSLog(@"after----begin----");
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2.0 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        // 2.0秒后异步追加任务代码到主队列，并开始执行
        NSLog(@"after---%@",[NSThread currentThread]);  // 打印当前线程
    });
    NSLog(@"after----end----");
}
```
> 打印结果
> 2018-04-20 14:24:19.624885+0800 GCD\_Test[11324:1481815] after----begin----
2018-04-20 14:24:19.625034+0800 GCD\_Test[11324:1481815] after----end----
2018-04-20 14:24:21.738247+0800 GCD\_Test[11324:1481815] after---\<NSThread: 0x60000006fdc0\>{number = 1, name = main}

### GCD只执行一次代码的方法：dispatch\_once

> 创建**单例**或者有整个程序运行过程中只执行一次的代码时可以用此方法，`dispatch_once`可以保证代码在程序运行过程中只执行一次，几时在多线程环境下，也可以保证线程安全。

```
#pragma mark - GCD只执行一次代码的方法：dispatch_once
- (void)gcdOnce {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        NSLog(@"----我只会执行一次----");
    });
}
```

### GCD快速迭代方法：dispatch\_apply

> `dispatch_apply`： 可以开启多个线程进行遍历，这在遍历数组上可以起到优化的作用，而且`dispatch_apply`会等全部任务执行完毕再继续执行后面的操作

```
- (void)gcdApply {
    NSLog(@"----apply--begin----");
    dispatch_queue_t globalQueue = dispatch_get_global_queue(0, 0);
    dispatch_apply([self.objArray count], globalQueue, ^(size_t index) {
        NSLog(@"-----%zd----%@",index, [NSThread currentThread]);
    });
    NSLog(@"----apply--end----");
}
```

> 2018-04-20 16:22:22.181731+0800 GCD\_Test[12647:1694230] ----apply--begin----
2018-04-20 16:22:22.181958+0800 GCD\_Test[12647:1694230] -----0----\<NSThread: 0x600000068840\>{number = 1, name = main}
2018-04-20 16:22:22.182013+0800 GCD\_Test[12647:1694425] -----2----\<NSThread: 0x604000264a80\>{number = 3, name = (null)}
2018-04-20 16:22:22.182036+0800 GCD\_Test[12647:1694427] -----3----\<NSThread: 0x60000047fbc0\>{number = 5, name = (null)}
2018-04-20 16:22:22.182038+0800 GCD\_Test[12647:1694428] -----1----\<NSThread: 0x6000004660c0\>{number = 4, name = (null)}
2018-04-20 16:22:22.182131+0800 GCD\_Test[12647:1694230] -----4----\<NSThread: 0x600000068840\>{number = 1, name = main}
2018-04-20 16:22:22.182305+0800 GCD\_Test[12647:1694427] -----6----\<NSThread: 0x60000047fbc0\>{number = 5, name = (null)}
2018-04-20 16:22:22.182306+0800 GCD\_Test[12647:1694425] -----5----\<NSThread: 0x604000264a80\>{number = 3, name = (null)}
2018-04-20 16:22:22.182341+0800 GCD\_Test[12647:1694428] -----7----\<NSThread: 0x6000004660c0\>{number = 4, name = (null)}
2018-04-20 16:22:22.182380+0800 GCD\_Test[12647:1694230] -----8----\<NSThread: 0x600000068840\>{number = 1, name = main}
2018-04-20 16:22:22.182465+0800 GCD\_Test[12647:1694425] -----9----\<NSThread: 0x604000264a80\>{number = 3, name = (null)}
2018-04-20 16:22:22.183692+0800 GCD\_Test[12647:1694230] ----apply--end----

### GCD队列组：dispatch\_group

> `dispatch_group`：GCD的队列组，可以调用`dispatch_group_async()`方法添加任务，然后按组执行

#### dispatch\_group\_notify

> 监听group中任务的完成状态，当组中所有任务完成是，调用`dispatch_group_notify`中的代码

```
#pragma mark - GCD任务组：dispatch_group_notify
- (void)groupNotify {
    NSLog(@"---group-begin--%@",[NSThread currentThread]);
    // 创建一个队列组
    dispatch_group_t queueGroup = dispatch_group_create();
    // 获取全局队列
    dispatch_queue_t globalQueue = dispatch_get_global_queue(0, 0);
    // 向队列组追加任务1
    dispatch_group_async(queueGroup, globalQueue, ^{
        [NSThread sleepForTimeInterval:2];
        NSLog(@"---任务1--耗时2秒---");
    });
    dispatch_group_async(queueGroup, globalQueue, ^{
        [NSThread sleepForTimeInterval:2];
        NSLog(@"---任务2--耗时1秒---");
    });
    //组中任务完成后再回到主线程调用
    dispatch_group_notify(queueGroup, dispatch_get_main_queue(), ^{
        NSLog(@"---group-end--%@",[NSThread currentThread]);
    });
}
```

> 2018-04-20 16:38:44.548952+0800 GCD\_Test[12770:1725716] ---group-begin--\<NSThread: 0x600000064000\>{number = 1, name = main}
2018-04-20 16:38:46.552524+0800 GCD\_Test[12770:1725781] ---任务2--耗时1秒---
2018-04-20 16:38:46.552524+0800 GCD\_Test[12770:1725782] ---任务1--耗时2秒---
2018-04-20 16:38:46.552835+0800 GCD\_Test[12770:1725716] ---group-end--\<NSThread: 0x600000064000\>{number = 1, name = main}


#### dispatch\_group\_wait

> 暂停阻塞当前线程，等待指定的group中完成任务后再继续往下执行。

```
- (void)groupWait {
    NSLog(@"----wait---begin---%@",[NSThread currentThread]);
    //创建一个队列组
    dispatch_group_t queueGroup = dispatch_group_create();
    // 获取全局队列
    dispatch_queue_t globalQueue = dispatch_get_global_queue(0, 0);
    dispatch_group_async(queueGroup, globalQueue, ^{
        [NSThread sleepForTimeInterval:4];
        NSLog(@"---任务1--耗时4秒---%@",[NSThread currentThread]);
    });
    dispatch_group_async(queueGroup, globalQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"---任务2--耗时1秒---%@",[NSThread currentThread]);
    });
    // 传-1需要等待，传0不需等待直接执行后面代码
    dispatch_group_wait(queueGroup, -1);
    NSLog(@"----wait---end---%@",[NSThread currentThread]);
}
```
> 2018-04-20 16:54:41.180500+0800 GCD\_Test[12970:1757380] ----wait---begin---\<NSThread: 0x604000079140\>{number = 1, name = main}
2018-04-20 16:54:42.181933+0800 GCD\_Test[12970:1757447] ---任务2--耗时1秒---\<NSThread: 0x604000276180\>{number = 3, name = (null)}
2018-04-20 16:54:45.182507+0800 GCD\_Test[12970:1757448] ---任务1--耗时4秒---\<NSThread: 0x60400027f640\>{number = 4, name = (null)}
2018-04-20 16:54:45.183019+0800 GCD\_Test[12970:1757380] ----wait---end---\<NSThread: 0x604000079140\>{number = 1, name = main}

#### dispatch\_group\_enter、dispatch\_group\_leave

> `dispatch_group_enter`类似于**对象引用计数**的`retain`每调用一次group的引用计数加一，`dispatch_group_leave`类似于**对象引用计数**的`release`没调用一次引用计数减一，当引用计数为0时，`dispatch_group_wait`会解除，然后调用`dispatch_group_notify`
> `dispatch_group_enter、dispatch_group_leave`的组合使用其实相当于`dispatch_group_async`

```
- (void)enterLeave {
    NSLog(@"--enter--begin--%@",[NSThread currentThread]);
    // 创建一个队列组
    dispatch_group_t queueGroup = dispatch_group_create();
    // 获取全局队列
    dispatch_queue_t globalQueue = dispatch_get_global_queue(0, 0);
    dispatch_group_enter(queueGroup);
    dispatch_async(globalQueue, ^{
        [NSThread sleepForTimeInterval:3];
        NSLog(@"---任务1--耗时3秒---%@",[NSThread currentThread]);
        dispatch_group_leave(queueGroup);
    });
    dispatch_group_enter(queueGroup);
    dispatch_async(globalQueue, ^{
        [NSThread sleepForTimeInterval:1];
        NSLog(@"---任务2--耗时1秒---%@",[NSThread currentThread]);
        dispatch_group_leave(queueGroup);
    });
    dispatch_group_notify(queueGroup, dispatch_get_main_queue(), ^{
        NSLog(@"----notify---end---%@",[NSThread currentThread]);
    });
    dispatch_group_wait(queueGroup, -1);
    NSLog(@"----wait--end--%@",[NSThread currentThread]);
}
```

> 2018-04-20 17:21:27.634479+0800 GCD\_Test[13266:1816278] --enter--begin--\<NSThread: 0x60400007ef00\>{number = 1, name = main}
2018-04-20 17:21:28.639462+0800 GCD\_Test[13266:1816382] ---任务2--耗时1秒---\<NSThread: 0x600000279140\>{number = 4, name = (null)}
2018-04-20 17:21:30.638860+0800 GCD\_Test[13266:1816380] ---任务1--耗时3秒---\<NSThread: 0x60400047fe00\>{number = 5, name = (null)}
2018-04-20 17:21:30.639271+0800 GCD\_Test[13266:1816278] ----wait--end--\<NSThread: 0x60400007ef00\>{number = 1, name = main}
2018-04-20 17:21:30.672949+0800 GCD\_Test[13266:1816278] ----notify---end---\<NSThread: 0x60400007ef00\>{number = 1, name = main}

### GCD信号量：dispatch\_semaphore
> GCD中的信号量是指**Dispatch Semaphore**，是持有计数的信号，编程人员可以通过控制`dispatch_semaphore_t`的信号计数，从而**控制线程代码的执行**

- 使用场景
	1. **保持线程同步，将异步执行任务转到同步执行**
	2. **保证线程安全，为线程加锁**
- `dispatch_semaphore`主要有三个函数：
	- `dispatch_semaphore_create(long value)`：初始化信号计数并返回一个`dispatch_semaphore_t`信号量
	- `dispatch_semphore_signal`：发送一个信号，让信号总量加1
	- `dispatch_semphore_wait`：当信号量为0时会一直等待（阻塞当前线程），否则正常运行，每运行一次会使信号总量减一。

> 注：**如果主线程调用时，可能会一直阻塞主线程**，所以在使用`dispatch_semaphore`前，一定要清楚自己操作的是哪个线程

#### Dispatch Semaphore 线程同步
> 比如：AFNetworking中的AFURLSessionManager.m里面的taskForKeyPath：方法，通过引入信号量的方式，等待异步执行结果，获取到tasks然后再做返回

```
- (NSArray *)tasksForKeyPath:(NSString *)keyPath {
    __block NSArray *tasks = nil;
    dispatch_semaphore_t semaphore = dispatch_semaphore_create(0);
    [self.session getTasksWithCompletionHandler:^(NSArray *dataTasks, NSArray *uploadTasks, NSArray *downloadTasks) {
        if ([keyPath isEqualToString:NSStringFromSelector(@selector(dataTasks))]) {
            tasks = dataTasks;
        } else if ([keyPath isEqualToString:NSStringFromSelector(@selector(uploadTasks))]) {
            tasks = uploadTasks;
        } else if ([keyPath isEqualToString:NSStringFromSelector(@selector(downloadTasks))]) {
            tasks = downloadTasks;
        } else if ([keyPath isEqualToString:NSStringFromSelector(@selector(tasks))]) {
            tasks = [@[dataTasks, uploadTasks, downloadTasks] valueForKeyPath:@"@unionOfArrays.self"];
        }

        dispatch_semaphore_signal(semaphore);
    }];

    dispatch_semaphore_wait(semaphore, DISPATCH_TIME_FOREVER);

    return tasks;
}
```

#### Dispatch Semaphore 线程安全（为线程加锁）

> 当多个线程访问**同一个类（方法、或对象）**，这个类始终能**表现出正确的行为**，那么这个类就是线程安全的。

- 比如：有10张火车票，两个售卖火车票的窗口，一个是北京窗口，一个是上海窗口。两个窗口同时售卖火车票，卖完为止。

- 在不考虑线程安全的情况下，代码如下：

```

#pragma mark - 不考虑线程安全的情况
- (void)shopTick {
//    NSLog(@"----0000----%@",[NSThread currentThread]);
    NSLog(@"----00000------%@",[NSThread currentThread]);
    self.tickCount = 10;
    // 创建上海队列
    dispatch_queue_t shangHai = dispatch_queue_create("com.Test.ShangHai", DISPATCH_QUEUE_SERIAL);
    // 创建背景队列
    dispatch_queue_t beiJing = dispatch_queue_create("com.Test.Beijing", DISPATCH_QUEUE_SERIAL);
    //卖票
    __weak typeof(self) weakSelf = self;
    dispatch_async(shangHai, ^{
        [weakSelf shop];
    });
    dispatch_async(beiJing, ^{
        [weakSelf shop];
    });
}

// 模拟售票
- (void)shop {
    while (1) {
        if (self.tickCount > 0) {  //如果还有票，继续售卖
            self.tickCount--;
            NSLog(@"%@", [NSString stringWithFormat:@"剩余票数：%d 窗口：%@", self.tickCount, [NSThread currentThread]]);
            [NSThread sleepForTimeInterval:0.2];
        } else { //如果已卖完，关闭售票窗口
            NSLog(@"所有火车票均已售完");
            break;
        }
    }
}
```
> 打印结果：
> 2018-04-21 16:29:18.566294+0800 GCD\_Test[20249:2807585] ----00000------\<NSThread: 0x60400007d800\>{number = 1, name = main}
2018-04-21 16:29:18.566609+0800 GCD\_Test[20249:2807784] 剩余票数：9 窗口：\<NSThread: 0x600000475480\>{number = 3, name = (null)}
2018-04-21 16:29:18.566615+0800 GCD\_Test[20249:2807785] 剩余票数：8 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:18.768913+0800 GCD\_Test[20249:2807784] 剩余票数：7 窗口：\<NSThread: 0x600000475480\>{number = 3, name = (null)}
2018-04-21 16:29:18.768913+0800 GCD\_Test[20249:2807785] 剩余票数：6 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:18.972331+0800 GCD\_Test[20249:2807784] 剩余票数：5 窗口：\<NSThread: 0x600000475480\>{number = 3, name = (null)}
2018-04-21 16:29:18.972333+0800 GCD\_Test[20249:2807785] 剩余票数：5 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:19.175883+0800 GCD\_Test[20249:2807784] 剩余票数：3 窗口：\<NSThread: 0x600000475480\>{number = 3, name = (null)}
2018-04-21 16:29:19.175888+0800 GCD\_Test[20249:2807785] 剩余票数：4 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:19.380030+0800 GCD\_Test[20249:2807785] 剩余票数：2 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:19.380030+0800 GCD\_Test[20249:2807784] 剩余票数：2 窗口：\<NSThread: 0x600000475480\>{number = 3, name = (null)}
2018-04-21 16:29:19.584104+0800 GCD\_Test[20249:2807784] 剩余票数：1 窗口：\<NSThread: 0x600000475480\>{number = 3, name = (null)}
2018-04-21 16:29:19.584104+0800 GCD\_Test[20249:2807785] 剩余票数：1 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:19.789115+0800 GCD\_Test[20249:2807784] 所有火车票均已售完
2018-04-21 16:29:19.789121+0800 GCD\_Test[20249:2807785] 剩余票数：0 窗口：\<NSThread: 0x6000004753c0\>{number = 4, name = (null)}
2018-04-21 16:29:19.992451+0800 GCD\_Test[20249:2807785] 所有火车票均已售完


- 考虑线程安全的情况下

```

- (void)shopTick {
//    NSLog(@"----0000----%@",[NSThread currentThread]);
    NSLog(@"----00000------%@",[NSThread currentThread]);
    self.tickCount = 10;
    // 初始化信号量
    _semaphore = dispatch_semaphore_create(1);
    // 创建上海队列
    dispatch_queue_t shangHai = dispatch_queue_create("com.Test.ShangHai", DISPATCH_QUEUE_SERIAL);
    // 创建背景队列
    dispatch_queue_t beiJing = dispatch_queue_create("com.Test.Beijing", DISPATCH_QUEUE_SERIAL);
    //卖票
    __weak typeof(self) weakSelf = self;
    dispatch_async(shangHai, ^{
        [weakSelf shop];
    });
    dispatch_async(beiJing, ^{
        [weakSelf shop];
    });
}

// 模拟售票
- (void)shop {
    while (1) {
        // 加锁，可以理解为如果正在售票则进行等待
        dispatch_semaphore_wait(_semaphore, -1);
        if (self.tickCount > 0) {  //如果还有票，继续售卖
            self.tickCount--;
            NSLog(@"%@", [NSString stringWithFormat:@"剩余票数：%d 窗口：%@", self.tickCount, [NSThread currentThread]]);
            [NSThread sleepForTimeInterval:0.2];
            //执行完一次售票操作，解锁
            dispatch_semaphore_signal(_semaphore);
        } else { //如果已卖完，关闭售票窗口
            NSLog(@"所有火车票均已售完");
            dispatch_semaphore_signal(_semaphore);
            break;
        }
    }
}
```
> 执行结果
> 2018-04-21 17:22:01.462763+0800 GCD\_Test[20462:2845077] ----00000------\<NSThread: 0x60000006cdc0\>{number = 1, name = main}
2018-04-21 17:22:01.463025+0800 GCD\_Test[20462:2845298] 剩余票数：9 窗口：\<NSThread: 0x60400026d800\>{number = 3, name = (null)}
2018-04-21 17:22:01.667396+0800 GCD\_Test[20462:2845296] 剩余票数：8 窗口：\<NSThread: 0x60400026de00\>{number = 4, name = (null)}
2018-04-21 17:22:01.870932+0800 GCD\_Test[20462:2845298] 剩余票数：7 窗口：\<NSThread: 0x60400026d800\>{number = 3, name = (null)}
2018-04-21 17:22:02.073737+0800 GCD\_Test[20462:2845296] 剩余票数：6 窗口：\<NSThread: 0x60400026de00\>{number = 4, name = (null)}
2018-04-21 17:22:02.278715+0800 GCD\_Test[20462:2845298] 剩余票数：5 窗口：\<NSThread: 0x60400026d800\>{number = 3, name = (null)}
2018-04-21 17:22:02.482867+0800 GCD\_Test[20462:2845296] 剩余票数：4 窗口：\<NSThread: 0x60400026de00\>{number = 4, name = (null)}
2018-04-21 17:22:02.686127+0800 GCD\_Test[20462:2845298] 剩余票数：3 窗口：\<NSThread: 0x60400026d800\>{number = 3, name = (null)}
2018-04-21 17:22:02.887050+0800 GCD\_Test[20462:2845296] 剩余票数：2 窗口：\<NSThread: 0x60400026de00\>{number = 4, name = (null)}
2018-04-21 17:22:03.091466+0800 GCD\_Test[20462:2845298] 剩余票数：1 窗口：\<NSThread: 0x60400026d800\>{number = 3, name = (null)}
2018-04-21 17:22:03.295766+0800 GCD\_Test[20462:2845296] 剩余票数：0 窗口：\<NSThread: 0x60400026de00\>{number = 4, name = (null)}
2018-04-21 17:22:03.499228+0800 GCD\_Test[20462:2845298] 所有火车票均已售完
2018-04-21 17:22:03.499457+0800 GCD\_Test[20462:2845296] 所有火车票均已售完



## 参考资料
> - [GCD-使用总结][1]
> - [iOS多线程：『GCD』详尽总结][2]
- [GCD官方文档][3]
- [iOS GCD之dispatch\_semaphore学习][4]

[1]:	https://www.jianshu.com/p/77c5051aede2
[2]:	https://www.jianshu.com/p/2d57c72016c6
[3]:	https://developer.apple.com/documentation/dispatch?language=objc
[4]:	https://www.jianshu.com/p/a84c2bf0d77b

