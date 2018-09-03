---
title: babel
layout: page
date: 2018-05-18 00:00
---

- babel的作用
>
>因为有的浏览器不支持es6、7的语法，所以babel可以将es6、7转译成es5，从而做到向下兼容的作用。

- babel安装

```
// 单个项目安装以来
npm install babel-cli --save-dev
// 全局安装
npm install babel-cli -g
```

- babel 使用

```
// 运行某个文件
babel-node test.js  
```

- babel预设
>
>.babelrc文件中做配置

```
// 安装命令
npm install babel-preset-es2015 --save-dev
// .babelrc文件中
{
    "presets": ["es2015"],
}
// package.json文件中
"scripts": {
    "start": "react-scripts start",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject",
    // 添加命令
    "build": "babel src -d dist -w"
  },
```

- babel插件

```
// 安装
npm install babel-plugin-add-module-exports --save-dev
// .babelrc文件中
{
    "presets": ["es2015"],
    "plugins": ["add-module-exports"]
}
```

- 使用babel转换react jsx语法

```
npm install babel-preset-react --save-dev
```

