---
title: JS基础
layout: page
date: 2018-04-02 00:00
---

[TOC]

##比较运算符

```
==   :   等于，只比较值
===  :   绝对等于，比较值和类型
!=   :   不等于，只比较值
!==  :   不绝对等于，值和类型有一个不相等或者两个都不相等为true
   ```

##typeof、Null、Undefined

###typeof操作符

- typeof可以用来检测变量的数据类型

```
typeof "John"                // 返回 string 
typeof 3.14                  // 返回 number
typeof false                 // 返回 boolean
typeof [1,2,3,4]             // 返回 object
typeof {name:'John', age:34} // 返回 object
```

###Null

- null是一个只有一个值的特殊类型，表示一个空对象引用。
- 可以设置一个null来清空对象

```
var person = null;           // 值为 null(空), 但类型为对象
```

###Undefined

- 在JS中，undefined是一个没有设置值的变量
- typeof一个没有值的变量会返回undefined
- 可以设置undefined来清空，类型为undefined

```
person = undefined;          // 值为 undefined, 类型是undefined
```

###Undefined和Null的区别

- null和undefined的值相等，但类型不相等

```
typeof undefined             // undefined
typeof null                  // object
null === undefined           // false
null == undefined            // true
```

##JS类型转换

- 通过使用JS函数转换
- 通过JS自身自动转换

###JS数据类型

- 在JS中有5中数据类型

    - string
    - number
    - boolean
    - object
    - function

- 3种对象类型

    - Object
    - Date
    - Array
    
- 2个不包含任何值的数据类型

    - null
    - undefined
    
###constructor属性

- constructor属性返回所有JS变量的构造函数

- 实例

```
"John".constructor                 // 返回函数 String()  { [native code] }
(3.14).constructor                 // 返回函数 Number()  { [native code] }
false.constructor                  // 返回函数 Boolean() { [native code] }
[1,2,3,4].constructor              // 返回函数 Array()   { [native code] }
{name:'John', age:34}.constructor  // 返回函数 Object()  { [native code] }
new Date().constructor             // 返回函数 Date()    { [native code] }
function () {}.constructor         // 返回函数 Function(){ [native code] }

```

###将数字/布尔值/日期转换为字符串

- 全局方法String()可以将数字／字母／变量／表达式转换为字符串

```
String(x)         // 将变量 x 转换为字符串并返回
String(false)       // 返回false
String(Date())  // 返回 Thu Jul 17 2014 15:38:19 GMT+0200 (W. Europe Daylight Time)
```
   
- 使用toString也能实现相同的效果

```
Date().toString()
false.toString()
(100 + 23).toString()
```

###将字符串／布尔值／日期转换为数字

- 全局方法Number() 可以将字符串转换为数字

    - 空字符串转换为0
    - 其他字符串转换为NaN(不是数字)

```
Number("3.14")    // 返回 3.14
//将布尔值转换为数字
Number(false)    //返回 0
Number(true)     //返回 1
//将日期转换为数字
d = new Date();
Number(d)          // 返回 1404568027739
//日期转换为为数字使用getTime()也有相同的效果
d.getTime()     //返回 1404568027739
```
- pareseFloat() 解析一个字符串，返回一个浮点数
- pareseInt() 解析一个字符串，返回一个整数

##JS正则表达式

> 正则表达可用于所有文本的搜索和替换操作

- 语法

```
/正则表达式主体／修饰符(可选)
```
   
- 实例

```
//runoob是正则表达式主体，i是一个修饰符（搜索时不区分大小写∫）
var patt = /runoob/i
```
   
###search()方法使用正则表达式

- search()方法用于检索字符串中指定字符串的，或检索与正则表达式相匹配的子字符串，并返回子串的起始位置。

- 实例

```
var str = "Visit Runoob!"
var n = str.search("Runoob");
//输出6
```
   
   
###search()方法使用字符串

- 实例

```
var str = "Visit Runoob!"
var n = str.search("Runoob");
//输出6
```

###replace()方法使用正则表达式

- 用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。

- 实例

```
//使用正则表达式，不区分大小写将字符串中的Microsoft替换为Runoob
var str = document.getElementById("demo").innerHTML;
var txt = str.replace("/microsoft/i", "Runoob");
```

###replace()方法使用字符串

- 实例

```
var str = document.getElementById("demo").innerHTML;
var txt = str.replace("Mirosoft","Runoob");
```
   
###正则表达式修饰符

- i : 执行不区分大小写的匹配
- g : 执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）
- m : 执行多行匹配


###正则表达式模式

- 方括号[]用于查找某个范围内的字符

    - [abc]  查找方括号之间的任何字符
    - [0-9]  查找任何从0至9的数字
    - (x|y)  查找任何以|分隔的选项
    - ...

- 元字符是拥有特殊含义的字符

    - \d    查找数字
    - \s    查找空白字符
    - \b    匹配单词边界
    - \uxxx 查找以十六进制数xxxx规定的Unicode字符
    - ...

- 量词

    - n+    匹配任何包含至少一个n的字符串
    - n*    匹配任何包含零个或多个n的字符串
    - n?    匹配任何包含零个或一个n的字符串
    - ...

###RegExp正则表达式对象

- RegExp对象是一个预定义了属性和方法的正则表达式对象。

####test()方法

> test()方法是一个正则表达式方法
> 用于检测字符串是否匹配某个正则表达式。如果字符串中含有匹配的文本，则返回true， 否则返回false


- 实例

```
var patt = /e/;
patt.test("The best things in life are free!");
//返回 true
```

####exec()方法

> exec()方法是一个正则表达式方法
> 用于检索字符串中的正则表达式的匹配
> 该函数返回一个数组，其中存放匹配的结果，如果未找到，则返回值为null


- 以下实例用于搜索字符串中的字母e

```
/e/.exec("The best things in life are free!");
//输出e
```

####完整的RegExp参考手册

[JavaScript RegExp 对象](http://www.runoob.com/jsref/jsref-obj-regexp.html)


##JS错误-throw、try和catch

- try   语句测试代码块的错误
- catch 语句处理错误
- throw 语句创建自定义错误

###try、catch使用实例

```
var text = "";
function message() {
try {
    addlert("Welcome guest!");
} catch(err) {
    txt = "本页又一个错误。\n\n"
    txt += "错误描述：" + err.message + ""\n\n;
}
}
```

###Throw使用实例

> throw语句允许我们创建或抛出异常

- 语法

```
throw exception
``` 
   
- 实例

```
   function myFunction() {
    var message, x;
    message = document.getElementById("message");
    message.innerHTML = "";
    x = document.getElementById("demo").value;
    try {
        if (x == "") throw "值为空";
        if (isNaN(x)) throw "不是数字";
        x = Number(x);
        if (x < 5)  throw "太小";
        if (x > 10) throw "太大";
    }
    catch (err) {
        message.innerHTML = "错误：" + err;
    }
   }
```
   

##JS变量提升

> JS 中，函数及变量的声明都将被提升到函数的最顶部。
> 因此，JS 中，变量可以先使用再声明。


- 实例

```
   x = 5; //设置x为5，在此之前并没有对变量x作声明
   
   elem = document.getElementById("demo"); //查找元素
   elem.innerHTML = x;
   
   var x; //在此声明变量x
```

###JS初始化变量，不会提升

- JS 只有声明的变量会提升，初始化的不会

- 实例

```
   elem = document.getElementById("demo");
   elem.innerHTML = x;
   var x = 7
   /／输出结果为Undefin
```


