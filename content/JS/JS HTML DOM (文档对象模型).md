---
title: DOM(文档对象模型)
layout: page
date: 2018-04-02 00:00
---

[TOC]
##DOM简介
- 当网页加载时，浏览器会创建页面的文档对象模型（DOM）。通过DOM，JS可以对HTML实现以下操作：
    
    > 1. 改变页面中的所有HTML元素。
    >2. 改变页面中的所有HTML属性。
    >3. 改变页面中的所有CSS样式。
    >4. 对页面中的所有事件作出反应。


     
##DOM HTML

###获得HTML属性的三种方式

- 通过id查找HTML元素
    
    ```
    <script>
    x=document.getElementById("intro");
    document.write("<p>文本来自 id 为 intro 段落: "    + x.innerHTML + "</p>");
    </script>
    ```
    
- 通过标签名找到元素
    
    ```
    <script>
    //查找id为main的元素
    var x=document.getElementById("main");
    //查找该元素中所有<p>元素，返回的为一个数组
    var y=x.getElementsByTagName("p");
    document.write('id="main"元素中的第一个段落为：' + y[0].innerHTML);
    </script>
    ```

- 通过类名查找元素

    ```
    <script>
x=document.getElementsByClassName("intro");
document.write("<p>文本来自 class 为 intro 段落: " + x[0].innerHTML + "</p>");
</script>
    ```

###改变HTML输出流

- js能够创建动态的HTML内容，比如动态获取当前时间等
- 使用document.write()可用于直接向HTML输出流写内容

    ```
<!DOCTYPE html>
<html>
<body>
<script>
document.write(Date());
</script>
</body>
</html>
    ```

###改变HTML内容

- 修改HTML内容最简单的方法是使用innerHTML属性

```
<script>
document.getElementById("p1").innerHTML="新文本!";
</script>
```

###改变HTML属性

- 改变HTML属性可以套用以下模板

    ```
    document.getElementById(id).attribute=新属性值
    ```
    
- 使用实例如下

    ```
<script>
//改变img的src属性
document.getElementById("image").src="landscape.jpg";
</script>
    ```

###改变HTML样式

- 改变HTML样式可以使用以下语法

    ```
document.getElementById(id).style.property=新样式
    ```

- 使用实例

    ```
    <script>
document.getElementById("p2").style.color="blue";
document.getElementById("p2").style.fontFamily="Arial";
document.getElementById("p2").style.fontSize="larger";
</script>
    ```
    
##HTML事件

###HTML事件类型

- HTML有诸如以下几种事件类型

> 1. 当用户点击鼠标时
2. 当网页已加载时
3. 当图像已加载时
4. 当鼠标移动到元素上时
5. 当输入字段被改变时
6. 当提交HTML表单时
7. 当用户触发按键时
8. ...
9. ..

###HTML事件属性

- 如需监听HTML元素的事件，可以使用事件属性进行分配

    ```
    //监听button元素的onClick事件
    <button onclick="displayDate()">点这里</button>
    ```

###使用HTML DOM来分配事件

- HTML DOM允许使用js来向HTML元素分配事件

    ```
<script>
//向button元素分配onClick事件
//并将displayDate函数分配给id=“myBtn”的HTML元素
document.getElementById("myBtn").onclick=function(){displayDate()};
</script>
    ```
    
####onload和onunload事件

- onload和onunload会在用户进入或离开页面时被触发，可用于检测访问者的浏览器类型和浏览器版本，并基于这些信息来加载网页的正确版本。也可用于处理cookie
    
    ```
//
<body onload="checkCookies()">
    ```

####onchange事件

- onchange事件常结合对输入字段的验证来使用

    ```
    //当用户改变输入字段的内容时，调用upperCase()函数
    <input type="text" id="fname" onchange="upperCase()">
```

####onmouseover和onmouseout事件

- onmouseover和onmouseout事件可用于在用户的鼠标移至HTML元素上方或移出元素时出发函数

    ```
<div onmouseover="mOver(this)" onmouseout="mOut(this)" style="background-color:#D94A38;width:120px;height:20px;padding:40px;">Mouse Over Me</div>
<script>
function mOver(obj){
	obj.innerHTML="Thank You"
}
function mOut(obj){
	obj.innerHTML="Mouse Over Me"
}
</script>
    ```
    
####onmouseDown、onmouseup、以及onclick事件

- 这三个事件构成了鼠标点击事件的所有部分、当点击鼠标按钮时，会出发onmousedown事件，当释放鼠标时会触发onmouseup事件，最后，当完成鼠标点击时，会触发onclick事件。

##DOM EventListener

###addEventListener()方法

#### 用处
    1. 用于向元素添加事件监听
    2. 一个元素可以添加多个监听事件
    3. 可以向同个元素添加多个同类型的监听事件，比如：两个onclick事件。
    4. 可以向任何DOM对象添加事件监听，不仅仅是HTML元素，比如：window对象。
    5. 可以更简单的控制事件（冒泡与捕获）
    6. 使用addEventListener()方法时，javascript从HTML标记中分离开来，可读性更强，在没有控制HTML标记时也可以添加事件监听。
    7. 可以使用removeEventListener()方法来移除事件的监听。

#### 语法


```
    //第一个参数是事件类型（如click或mousedown）
    //第二个参数是事件触发后调用的函数
    //第三个参数是布尔值，用于描述事件是冒泡还是捕获。该参数是可选的，默认为false，冒泡传递
    element.addEventListener(event, function, useCapture);
```
    
- 实例

    ```
//当用户点击按钮时触发监听事件
document.getElementById("myBtn").addEventListener("click", displayDate);
    ```
    

####向原元素添加事件句柄

- 实例1: 当用户点击元素时弹出“hello world”

    ```
element.addEventListener("click", function(){ alert("Hello World!"); });
    ```
    
- 实例2: 使用函数名，来引用外部函数

    ```
    //当用户点击元素时弹出helloworld
    element.addEventListener("click", myFunction);
function myFunction() {
    alert ("Hello World!");
}
    ```


####向同一元素添加多个事件句柄

- 向同个元素添加多个同种类型事件,注： 不会覆盖已存在的事件

   ```
   element.addEventListener("click", myFunction);
element.addEventListener("click", mySecondFunction);
   ```
   
- 向同个元素添加多个不同类型事件

   ```
   element.addEventListener("mouseover", myFunction);
element.addEventListener("click", mySecondFunction);
element.addEventListener("mouseout", myThirdFunction);
   ```


####向window对象添加事件句柄

- addEventListener()方法允许向HTML DOM对象添加事件监听。HTML DOM对象如： HTML元素，HTML文档，window对象，或者其他支出的事件对象如： xmlHttpRequest对象。

   ```
   //当用户重置窗口大小时添加事件监听
   window.addEventListener("resize", function(){
    document.getElementById("demo").innerHTML = sometext;
});
   ```
   



####传递参数

- 当传递参数时，使用“匿名函数”调用带参数的函数

   ```
   element.addEventListener("click", function(){ myFunction(p1, p2); });
   ```





####事件的冒泡传递和捕获传递

- 事件传递有两种方式： 冒泡或捕获，这两种事件传递定义了元素事件触发的顺序。
- 比如： p元素插入到 div 元素中，用户点击 p 元素, 哪个元素的 "click" 事件先被触发呢？
- 在**冒泡传递中**内部事件会先被触发，然后再触发外部元素。
- 在**捕获传递中**外部元素事件会先被触发，然后才会触发内部元素的事件




####removeEventListener()方法

- removeEventListener()方法移除由addEventListener()方法添加事件的句柄。

   ```
   element.removeEventListener("mousemove", myFunction);
   ```

