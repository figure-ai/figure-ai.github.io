---
title: RN常用指令
layout: page
date: 2018-04-02 00:00
---


```
//查看当前reactNative版本
react-native --v
//更新全局rn到最新版
sudo npm update -g react-native-cli
//查看服务器端rn各版本的信息
npm info react-native
// 根据配置的package.json给下载的工程项目下载安装rn环境
npm install
// 根据package.json配置的rn版本，更新rn环境代码
react-native upgrade
// 运行ios工程
react-native run-ios
// 在对应模拟器下运行项目
react-native run-ios --simulator "iPhone 7"
// 运行android工程
react-native run-android
// 初始化一个工程，下载react-native 的所有源代码和依赖包
react-native init 工程名
// 项目降级或升级到指定版本，记得之后要运行一下react-native upgrade 更新一下项目依赖等
npm install --save react-native @0.18
// 安装某个lib到项目中
npm install react-native-storage --save
```

