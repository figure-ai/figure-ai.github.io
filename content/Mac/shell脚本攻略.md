---
title: shell脚本攻略
layout: page
date: 2018-05-18 00:00
---


## 变量与环境变量

- 打印彩色输出

```
//40:黑色  41:红色 42：绿色 43：黄色 44：蓝色 45：洋红 46：青色 47：白色
echo -e "\e[1;41m Green background "
```

- 在bash中每一个变量都是字符串，赋值前不需要声明类型

- 给变量赋值

```
var = "value"
```

- 获取变量

```
echo "$var"
```

- **环境变量是未在当前进程中定义，而从父进程中继承过来的**

- **export**用来设置环境变量，如下所有在当前执行的shell都会继承HTTP_PATH这个变量

```
HTTP_PATH = "http://192.168.0.2"
export HTTP_PATH
```

- 获取字符串长度

```
length = $(#var)
```

- 获取当前使用的是哪种shell

```
echo $SHELL
```

- 判断是否为超级用户

```
if [ $UID -ne 0 ]
then
    echo no root user
else
    echo root user
fi
```

##数学运算

- 使用let进行整数加减乘除

```
value1=20
value2=5
# let result=value2+value1
# let result=value1-value2
# let result=value1*value2
# let result=value1/value2
# let value1--
# let value1++
echo $result
```

- 使用bc进行浮点数运算

```
# bc浮点数计算
echo "4*0.56" | bc
no=54
result=` "$no * 1.5" | bc`
echo $result
# scale设定小数精度
echo "scale=2;13/8" | bc
# obase/ibase进制转换
no=100
echo "obase=2;$no" | bc
no=1100100
echo "obase=10;ibase=2;$no" | bc
```

## 文件内容的操作

 ```
 # 清除文件的内容，写入新的内容
 echo "This is sample text " > temp.txt
 
 # 拼接新内容
 echo "This is sample text2" >> temp.txt
 ```

