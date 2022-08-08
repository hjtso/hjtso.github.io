---
title: Mac OS X设置默认Python版本
date: 2017-01-04 12:59:30
tags: Prose
category: Prose
---
想在Mac OS X设置默认Python版本，系统自带是2.7，自装的是3.5. 以下方法：

### Method 1.修改链接
先备份原来的python链接，再把3.5的解释器做一个链接到原目录下，适当修改相关路径：
``` bash
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.5/bin/python3.5 /usr/local/bin/python
```
但是建议不要用链接方法，因为可能会导致想调用自带python的软件出现问题。

### Method 2.修改环境变量
可以修改~/.bash_profile，修改path variable虽然比较安全。
找不到.bash_profile文件的可以使用find查找：
``` bash
find / -name .bash_profile 
```
然后在bash_profile文件添加下行（具体path取决python3安装路径）：
``` bash
alias python="/System/Library/Frameworks/Python.framework/Versions/3.5/bin/python3.5"
```
然后重启一下Terminal。

### Method 3.直接加版本号
最便捷的方法是：
如果想用系统自带python2.7，就在命令行输入“python”；
如果想用python3.5，就在命令行输入“python3”。
吾辈最终还是觉得这方法最合适。


### Python路径相关：
① Mac自带的python环境在(Python 2.7.10)：
``` bash
/System/Library/Frameworks/Python.framework/Versions/2.7
```
② 用户安装的python环境在(Python 3.5.2)：
``` bash
/Library/Frameworks/Python.framework/Versions/3.5
```
③ Mac自带的python解释器默认路径在(Python 2.7.10)：
``` bash
/usr/bin/python2.7
```
④ 用户安装的python解释器默认路径在(Python 3.5.2)：
``` bash
/usr/local/bin/python3.5
```
