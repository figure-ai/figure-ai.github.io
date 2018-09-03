---
title: JS浏览器BOM
layout: page
date: 2018-04-02 00:00
---

[TOC]
##JS Window-浏览器对象模型

- 浏览器对象模型（BOM）使JS有能力与浏览器对话
- 由于现代浏览器几乎实现了JS交互性方面的相同方法和属性，因此常被认为是BOM的方法和属性。


###Window对象

- 所有浏览器都支持Window对象，它表示浏览器窗口。
- 所有JS全局对象、函数以及变量均会自动成为window对象的成员。
- 全局变量是window对象的属性
- 全局函数是window对象的方法
- HTML DOM的document也是window对象的属性之一
    
       ```
   window.document.getElementById("header");
        ```
           


###Window尺寸

- 有三种方法可以确定浏览器窗口的尺寸

- 对于IE、Chrome、Firefox、Opera、Safari
    
    - window.innerHeight - 浏览器窗口的内部高度（包括滚动条）
    - window.innerWidth - 浏览器窗口的内部宽度（包括滚动条）
    
    
- 对于IE8、7、6、5

    - document.documentElement.clientHeight
    - document.documentElement.clientWidth
    
- 或者

    - document.body.clientHeight
    - document.body.clientWidth

- 实例：获取浏览器高度宽度（涵盖所有浏览器）

    ```
var w=window.innerWidth
|| document.documentElement.clientWidth
|| document.body.clientWidth;
var h=window.innerHeight
|| document.documentElement.clientHeight
|| document.body.clientHeight;
   ```

###其他Window方法

- window.open() - 打开新窗口
- window.close() - 关闭当前窗口
- window.moveTo() - 移动当前窗口
- window.resizeTo() - 调整当前窗口


##JS 获取有关用户屏幕信息

- window.screen 对象包含有关用户屏幕的信息
- window.screen 对象在编写时可以不使用window这个前缀
    - screen.avaliWidth - 可用的屏幕宽度
    - screen.avaliHeight - 可用的屏幕高度
    
    
###Window Screen 可用宽度

- screen.avaliWidth 属性返回访问者屏幕的宽度，以像素为单位，减去界面特性，比如窗口任务栏这些。

   ```
   <script>
document.write("可用宽度: " + screen.availWidth);
</script>
   ```
   
   
   


###Window Screen 可用高度

- screen.avaliHeight 属性返回访问者屏幕的高度，以像素为单位，减去界面特性，比如窗口任务栏这些。

   ```
   <script>
document.write("可用高度: " + screen.availHeight);
</script>
   ```
   
   
##JS Window Location对象

- window.location 对象用于获得当前页面的地址URL，并把浏览器重定向到新的界面。编写时可不写window前缀。
- location的一些实例
    
    - location.hostname 返回web主机的域名
    - location.pathname 返回当前页面的路径和文件名
    - location.port 返回web主机的端口（80或443）
    - location.protocol 返回所使用的web协议（http://或https://）

###获取URL（href）
     
- location.href 属性返回当前页面的url

   ```
<script>
//输出当前页面的URL
document.write(location.href);
</script>
//输出： http://www.runoob.com/js/js-window-location.html
   ```
        
###获取URL路径名（pathname）
    
- location.pathname 属性返回URL的路径名

  ```
<script>
document.write(location.pathname);
</script>
//输出： /js/js-window-location.html
   ```
        

###加载新文档（网页）
    
- location.assign() 方法加载新的文档

       ```
//加载一个新文档，即打开http://www.w3cschool.cc这个页面
   <html>
<head>
<script>
function newDoc()
  {
  window.location.assign("http://www.w3cschool.cc")
  }
</script>
</head>
<body>
<input type="button" value="Load new document" onclick="newDoc()">
</body>
</html>
        ```
        
        





##JS 获取历史记录

- window.history 对象包含浏览器的历史，编写时可以省略window前缀

###后退网页

- history.back() 加载历史记录的前一个URL，与浏览器点击后退按钮效果相同

   ```
   <html>
<head>
<script>
function goBack()
  {
  window.history.back()
  }
</script>
</head>
<body>
<input type="button" value="Back" onclick="goBack()">
</body>
</html>
   ```
   
###前进网页

- history forward() 方法加载历史列表的下一个URL，与在浏览器中点击前进按钮效果相同

   ```
       <html>
    <head>
    <script>
    function goForward()
      {
      window.history.forward()
      }
    </script>
    </head>
    <body>
    <input type="button" value="Forward" onclick="goForward()">
    </body>
    </html>
   ```

## JS获取浏览器信息

- window.navigator 对象包含有关访问者浏览器的信息，编写时可以省略window前缀

   ```
   <div id="example"></div> 
<script> 
txt = "<p>浏览器代号: " + navigator.appCodeName + "</p>"; 
txt+= "<p>浏览器名称: " + navigator.appName + "</p>"; 
txt+= "<p>浏览器版本: " + navigator.appVersion + "</p>"; 
txt+= "<p>启用Cookies: " + navigator.cookieEnabled + "</p>"; 
txt+= "<p>硬件平台: " + navigator.platform + "</p>"; 
txt+= "<p>用户代理: " + navigator.userAgent + "</p>"; 
txt+= "<p>用户代理语言: " + navigator.systemLanguage + "</p>"; 
document.getElementById("example").innerHTML=txt; 
</script> 
   ```

- 需要注意的是，navigator对象的信息具有误导性，因为：
    
    - navigator数据可悲浏览器使用者更改
    - 一些浏览器对测试站点会识别错误
    - 浏览器无法报告晚于浏览器发布的新操作系统

- 使用对象检测可以用来检测不同的浏览器
    
    - 例如，只有Opera支持属性window.opera，因此可以识别出Opera
        
           ```
           if(window.opera) {
            //此为Opera浏览器
           }
           ```


##JS 弹窗

- JS中可以创建三种消息框：警告框、确认框、提示框


###警告框

- 当警告框出现后，用户需要点击确定按钮才能继续进行操作
- 语法：

   ```
   //这里也可以省略window前缀，直接使用alert()方法
   window.alert("sometext");
   ```
   
- 实例

   ```
   <!DOCTYPE html>
<html>
<head>
<script>
function myFunction()
{
    alert("你好，我是一个警告框！");
}
</script>
</head>
<body>
<input type="button" onclick="myFunction()" value="显示警告框">
</body>
</html>
   ```
   
###提示框

- 出现提示框时，用户需要输入某个值，然后点击确定，返回值为输入的值，点击取消，返回值为null

- 语法

   ```
window.prompt("sometext","defaultvalue");
   ```
   
- 实例

   ```
   var person=prompt("请输入你的名字","Harry Potter");
if (person!=null && person!="")
{
    x="你好 " + person + "! 今天感觉如何?";
document.getElementById("demo").innerHTML=x;
}
   ```

###确认框

- 语法

   ```
   //这里的window可以省略
   window.confirm("sometext");
   ```

- 实例

    ```
   var r=confirm("按下按钮");
if (r==true)
{
    x="你按下了\"确定\"按钮!";
}
else
{
    x="你按下了\"取消\"按钮!";
}
   ```

##JS计时事件

- setInterval() 间隔指定的毫秒数不停地执行指定的代码。
- setTimeOut() 经过指定的毫秒数后执行指定的代码
- 注： 这是HTML DOM Window对象的两个方法


###setInterval()方法

- 间隔指定的毫秒数不停地执行指定的代码
- 语法

   ```
   window.setInterval("javascript function", milliseconds);
   ```

- 实例1

   ```
   //每3秒弹出一个hello
   setInterval(function(){alert("Hello")},3000);
   ```

- 实例2: 实时显示当前时间

   ```
   var myVar=setInterval(function(){myTimer()},1000);
function myTimer()
{
var d=new Date();
var t=d.toLocaleTimeString();
document.getElementById("demo").innerHTML=t;
}
   ```

###clearInterval()方法

- clearInterval()方法用来停止setInterval()方法执行的函数代码
- 语法

   ```
   clearInterval(intervalVariable)
   ```

- 实例

   ```
   <p id="demo"></p>
<button onclick="myStopFunction()">Stop time</button>
<script>
var myVar=setInterval(function(){myTimer()},1000);
function myTimer()
{
var d=new Date();
var t=d.toLocaleTimeString();
document.getElementById("demo").innerHTML=t;
}
function myStopFunction()
{
clearInterval(myVar);
}
</script>
   ```

###setTimeout()方法

- 经过多少时间后执行某个函数

- 语法

   ```
   window.setTimeout("javascript 函数",毫秒数);
   ```

- 实例

   ```
   //等待3秒，弹出hello
   setTimeout(function(){alert("hello")},3000);
   ```
   
###clearTimeout()

- 用于停止执行setTimeout()方法的函数代码，如果函数未执行

- 语法

   ```
   clear.Timeout(timeoutVariable)
   ```
   
- 实例

   ```
   var myVar;
function myFunction()
{
myVar=setTimeout(function(){alert("Hello")},3000);
}
function myStopFunction()
{
clearTimeout(myVar);
}
   ```

##JS Cookie

- Cookie用于存储web页面的用户信息  
    
    - 当用户访问web页面时，他的名字可以记录在cookie中
    - 在用户下一次访问页面时，可以在cookie中读取用户访问记录。

- cookie以建值对形式存在
    
    ```
   username = Lch
   ```
   
- JS可以使用document.cookie属性来创建、读取、及删除cookie

###使用JS创建cookie

- 创建cookie

   ```
   document.cookie = "username=LCH"
   ```
- 为cookie添加一个过期时间（以UTC或GMT时间），默认情况下，cookie在浏览器关闭时删除。

   ```
   document.cookie="username=LCH; expires=Thu,  18 Dec 2013 12:00:00 GMT"
   ```

- 使用path参数告诉浏览器cookie的路径，默认情况下，cookie属于当前页面

   ```
   document.cookie="username=John Doe; expires=Thu, 18 Dec 2013 12:00:00 GMT; path=/";
   ```
   
- 实例： 设置cookie值的函数

   ```
   //cname: cookie的名称；cvalue: cookie的值；expires: cookie的过期时间
   function setCookie(cname, cvalue, exdays)
   {
    var d = new Date();
    d.setTime(d.getTime()+(exdays*24*60*60*1000));
    var expires = "expires="+d.toGMTString();
    document.cookie = cname + "=" + cvalue + "; " + expires;
   }
   ```
   
###使用JS读取cookie

- 在JS中可以使用以下代码来读取cookie

   ```
   var x = document.cookie;
   ```

- 实例： 获取cookie值的函数

   ```
function getCookie(cname)
{
  var name = cname + "=";
  var ca = document.cookie.split(';');
  for(var i=0; i<ca.length; i++) 
  {
    var c = ca[i].trim();
    if (c.indexOf(name)==0) return c.substring(name.length,c.length);
  }
  return "";
}
   ```
   
###使用JS修改cookie

- 在JS中，修改cookie类似于创建cookie，修改之后旧的cookie将被覆盖

   ```
   document.cookie="username=John Smith; expires=Thu, 18 Dec 2013 12:00:00 GMT; path=/";
   ```

###使用JS删除cookie

- 删除cookie只需设置参数为以前的时间即可

   ```
   document.cookie = "username=; expires=Thu, 01 Jan 1970 00:00:00 GMT";
   ```
   

###检测cookie值的函数

- 以下是一个检测cookie是否创建的函数，如果设置cookie则显示问候信息，如果没有设置 cookie，将会显示一个弹窗用于询问访问者的名字，并调用 setCookie 函数将访问者的名字存储 365 天：
   ```
function checkCookie()
{
  var username=getCookie("username");
  if (username!="")
  {
    alert("Welcome again " + username);
  }
  else 
  {
    username = prompt("Please enter your name:","");
    if (username!="" && username!=null)
    {
      setCookie("username",username,365);
    }
  }
}
   ```



