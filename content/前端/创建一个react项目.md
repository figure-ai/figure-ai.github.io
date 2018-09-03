---
title: 创建一个react项目
layout: page
date: 2018-05-18 00:00
---


# 创建一个react项目

##安装node.js和yarn
1. 安装node.js
2. 安装yarn（node.js的包管理工具）
3. 执行yarn init 初始化项目(会生成package.json文件） - yarn常用指令

```
yarn init
yarn add xxx@xxx
yarn add xxx@xxx --dev
yarn global add xxx@xxx
yarn remove xxx@xxx
yarn run
```

##配置webpack

>
>一个前端资源加载/打包工具

- 使用webpack处理的文件类型

```
html         ->  html-webpack-plugin
脚本          ->  babel + babel-preset-react
样式          ->  css-loader + scss-loader
图片/字体      ->   url-loader + file-loader 
```
- webpack 常用模块

```
html-webpack-plugin                 html单独打包成文件
extract-text-webpack-plugin         样式打包成单独文件
CommonsChunkPlugin                  提出通用模块（webpack自带，当某一模块引用超过一定次数就会被抽到这个模块）
webpack-dev-server                  能让我们在浏览器里通过http的形式访问编译好的文件，同时还可以动态刷新代码，实现路径转发
```

- 使用yarn安装webpack

```
yarn add webpack@x.x.x --dev
```

- 根目录下新建webpack配置文件（webpack.config.js）

- 没有使用全局安装的webpack执行的打包命令

```
node_module/.bin/webpack
```

- 安装html-webpack-plugin打包html文件

```
yarn add html-webpack-plugin@x.x.x --dev
```

- 安装babel-load处理es6语法

```
yarn add babel-loader@x.x.x babel-core@x.x.x babel-preset-env@x.x.x webpack --dev
```

- 安装babel-preset-react

```
yarn add babel-preset-react@x.x.x --dev
```

- 安装react、react-dom

```
yarn add react@x.x.x react-dom@x.x.x 
```

- 安装css-loader、style-loader处理css文件

```
yarn add style-loader@x.x.x --dev
yarn add css-loader@x.x.x --dev
```

- 安装extract-text-webpack-plugin

```
yarn add extract-text-webpack-plugin@x.x.x --dev
```

- 安装scass-loader node-scss

```
yarn add sass-loader@x.x.x --dev
yarn add node-sass@x.x.x --dev
```

- 安装url-loader、file-loader处理图片

```
yarn add url-loader@x.x.x --dev
yarn add file-loader@x.x.x --dev
```

- 安装字体库

```
yarn add font-awesome
```

