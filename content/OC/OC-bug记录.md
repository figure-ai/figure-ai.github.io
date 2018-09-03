---
title: OC-bug记录
layout: page
date: 2018-05-14 00:00
---
# bug记录

- `iOS ：undefined symbols for architecture x86_64`
    
    ```
    缺少静态库，把相应的静态库添加到Link Binary With Libraries即可
    ```
- 某个frameWork报:`Reason: image not found`，将对应的库添加到Embedded Binaries 并且clean项目

![](https://ws3.sinaimg.cn/large/None.jpg)

- 打包上传appstroe报`Invalid Bundle - A nested bundle contains simulator platform listed in CFBundleSupportedPlatforms Info.plist key.`
    
    ```
    全局搜索CFBundleSupportedPlatforms，将有用到此key的info文件的值改为iPhoneOS，
    ```
    
- 使用UICollectionView时出现没有渲染cell的情况，创建的时候没有调用到cellForItemAtIndexPath:方法

```
//把UICollectionViewFlowLayout写成UICollectionViewLayout  
UICollectionViewFlowLayout *layout = [[UICollectionViewFlowLayout alloc] init];
RCTCollectionView *collection = [[RCTCollectionView alloc] initWithFrame:CGRectZero collectionViewLayout:layout];
```

- 打包上传报`No iTunes Connect access for the team`

```
退出账号
重启xcode
登录账号
上传
```

- 打包报`no space left on device`

```
磁盘空间不足，清理内存，重启xcode
```


