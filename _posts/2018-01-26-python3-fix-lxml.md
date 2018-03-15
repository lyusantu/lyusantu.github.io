---
title: Python3无法使用lxml的解决方案
date: 2018-01-25 07:04:02
---

## 前言

`python`版本`3.6.3`，安装了`bs4`，运行代码报错：`Couldn't find a tree builder with the features you requested: lxml. Do you need to install a parser library? `

## 解决方案

### 安装wheel

打开控制台窗口，在命令行中执行如下命令：

```
pip install wheel
```

### 安装lxml

`wheel`安装完成后，在 [www.lfd.uci.edu/~gohlke/pythonlibs/](www.lfd.uci.edu/~gohlke/pythonlibs/) 中下载对应版本的`lxml`的后缀为`.whl`的文件，下载完成后，打开下载目录，按住`Shift`键，然后鼠标右键点击选择**在此处打开命令行窗口**：

![](http://oih7sazbd.bkt.clouddn.com/Fl9jAetmyJgnI2yFQ3wREQBTneKK)

在打开的命令行窗口中输入如下命令：

![](http://oih7sazbd.bkt.clouddn.com/FtVFyAZel7OWttnsBetaVWWP_J_X)


### 验证结果
上述安装完成后，在任意地方打开命令行窗口，执行如下命令：

![](http://oih7sazbd.bkt.clouddn.com/Fi-EJXShhX7wRcJ1fGJysZ5bP583)

如果成功执行，没有报错，就表示安装成功了！









