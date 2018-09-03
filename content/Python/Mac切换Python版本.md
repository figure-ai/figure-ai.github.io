---
title: Mac切换Python版本
layout: page
date: 2018-04-26 00:00
---

#Mac切换Python版本

1. 查看本机python目录

```
which python3.6
// ／usr/local/bin/python3.6
```

2. 修改bashrc配置文件
    
```
vi ~/.bashrc
```

3. 想要为某个特定用户修改 Python 版本，只需要在其 home 目录下创建一个 alias(别名) 即可。打开该用户的 ~/.bashrc 文件，添加新的别名信息来修改默认使用的 Python 版本。
    
```
alias python='/usr/local/bin/python3.6'
```
    
4. 验证
    
```
python
```

