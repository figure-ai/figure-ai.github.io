---
title: html
layout: page
date: 2018-05-18 00:00
---

- html

```
<!--文本符-->
<p> 段落文本 </p>

<!--标题符-->
<h1> 一级标题 <h1/>
...
<h6> 六级标题 <h6/>

<!--文本粗体-->
<strong> 文本粗体 </strong>

<!--文本斜体-->
<em> 文本斜体 <em/>

<!--自定义的文本样式-->
<span> 设置自定义的文本样式 </span>

<!--引用文本-->
<q> 引用文本（为文本添加一个双引号） </q>

<!--长文本的引用-->
<blockquote> 长文本的引用（诗词歌赋等） </blockquote>

<!--文本换行符-->
<br/> 

<!--空格符-->
&nbsp;  

<!--分割线  默认粗灰色-->
<hr/> 
  
<!--地址标签-->
<address>地址标签  默认样式为斜体</address>

<!--代码标签-->
<code> 代码标签  如果多行的话可以使用<pre> </code>

<!--代码标签-->
<pre> 代码标签 多行 </pre>

<!--无序列表-->
<ul>
    <li>列表项一  默认样式为自带一个原点</li>
    ...
</ul>

<!--有序列表 默认样式1.2.3....-->
<ol>
    <li>信息1</li>
    <li>信息2</li>
    <li>信息3</li>
</ol>

<!--布局块 类似iOS的view控件-->
<div></div>

<!--表格 默认没有表格线-->
<table summary="表格的简介，不会在浏览器上显示">
    <!--表格的标题-->
    <caption>表格的标题文本</caption>
    <!--加了tbody可以使表格加载多少显示多少，否则需等待整个表格加载完成再显示，-->
    <tbody>
        <!--表格的一行-->
        <tr>
            <!--表格的表头-->
            <th></th>
            <th></th>
        </tr>
        <tr>
            <!--表格的普通行-->
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

<!--链接 默认颜色为蓝色有下划线-->
<a href="目标网址" target="_blank" title="鼠标滑过显示的文本">链接显示的文本</a>

<!--在新建窗口打开链接-->
<a target="_blank"></a>

<!--使用mailto链接邮件地址 -->
<a href="mailto:xxx@qq.com;xxx@gmail.com?cc=抄送地址&bcc= 密件抄送地址&subject=邮件主题&body=邮件内容"></a>

<!--图片-->
<img src="图片地址" alt="下载失败时替换的文本" title="鼠标滑过时显示的文本，图片可见时有效"></img>

<!--表单 把用户输入的数据传送到服务器-->
<!--注：所有表单控件（文本框、文本域、按钮、单选框、复选框等）都必须放在 <form></form> -->
<form method="传送方式" action="服务器的文件"> </form>
<!--form 使用示例-->
<form method="post" action="save.php">
      <label for="username">用户名:</label>
      <input type="text"  name="username" id="username" value="" /><br>
      <label for="pass">密码:</label>
      <input type="password"  name="pass" id="pass" value="" /><br>
      <input type="submit" value="确定"  name="submit" />
      <input type="reset" value="重置" name="reset" />
</form>

<!--文本输入框-->
<input type="text/password" name="为文本框命名，以备后台程序ASP 、PHP使用" value="文本框的默认值">

<!--文本框-多行输入-->
<textarea cols="50" rows="10"></textarea>

<!--单选框 默认样式为原点-->
<input type="radio" value="提交到服务器的值" name="gender-man" />

<!--复选框 默认样式为小框 打钩-->
<input type="checkbox" checked="设为checked时默认选中该项" value="提交到服务器的值" name="gender-woman" />

<!--下拉列表框-->
<!--设置multipe时为下拉多选框-->
<select multiple="multiple">
    <!--列表选项-->
    <option value="看书">看书</option>
    <!--设为selected时默认选中-->
    <option value="旅游" selected = "selected">旅游</option>
    <option value="运动">运动</option>
    <option value="购物">购物</option>
</select>

<!--提交按钮 提交表单信息到服务器-->
<input type="submit" value="按钮文本">

<!--重置按钮 重置表单信息-->
<input type="reset" value="重置">

<!--表单中的label标签-->
<label for="点击标签响应的控件id">
```

- 元素分类

```
常用的块状元素有：

<div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>

常用的内联元素有：

<a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>

常用的内联块状元素有：

<img>、<input>
```



