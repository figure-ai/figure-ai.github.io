---
title: RN-bug记录
layout: page
date: 2018-04-02 00:00
---


1. 运行react-native run-ios时报`make sure you're running a packager server or have included`
    
    - 原因： 由于shadowsocks的网络代理设置为了全局代理（去翻墙了）导致了之前可以正常连接到本地的package的server需要绕道到国外服务器，再去连接本地的，所以无法正常访问了。
    - 解决方法： 换为自动代理或者直接关闭shadowsocks

2. 运行项目报；`DeviceInfo native module is not installed correctly`
    
    - 原因：当你停止运行一个xcode RN项目时，它不会停止终端中的打包程序，并且当您在xcode中启动/打开另一个RN项目时，只需检查打包程序是否正在运行，则不检查它是否与当前项目相关联。因此，请确保停止终端中的所有打包程序实例，然后再次打开RN项目。
    - 解决方法：关闭终端，重新运行项目

3.yoga模块报错`Implicit conversion loses integer precision: 'size_type' (aka 'unsigned long') to 'const uint32_t' (aka 'const unsigned int')`

- 解决方法：在podfile文件中加入以下代码
    
    ```
    post_install do |installer|
    installer.pods_project.targets.each do |target|
        if target.name == 'yoga'
            target.build_configurations.each do |config|
                config.build_settings['GCC_TREAT_WARNINGS_AS_ERRORS'] = 'NO'
                config.build_settings['GCC_WARN_64_TO_32_BIT_CONVERSION'] = 'NO'
            end
        end
    end
end
    ```
    
- `tries to require 'react-native', but there are several files providing this module. You can delete or fix them:`

 ```
 重复添加依赖库，把profile中添加的库改成手动添加即可。
 ```


