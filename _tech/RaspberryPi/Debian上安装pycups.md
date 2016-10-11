---
title: "如何在Debian上安装pycups"
permalink: /tech/debian-pycups/
modified: 2016-10-11T22:40:02-04:00
---

{% include base_path %}
{% include toc %}
## 简介
----
在Linux的学习和使用中，总是有很多零碎的知识需要总结，也许大牛们早已经整清楚是怎么回事了，请勿喷哈，嘿嘿嘿。。。

这一次就是在公司的做一个应用时遇到的问题，需要用树莓派连接一个打印机，在程序中需要控制打印的内容。因为python比较快捷，所以就使用了pycups。


## 关于CUPS
----
CUPS全称为Common UNIX Printing System 是Unix环境下的通用打印系统。Unix/Linux下打印总是有许多限制。但若安装了它，将会得到一个完整的打印解决方案。

在UNIX/Linux 下打印的方法很久以来都是用lpd（命令行方式的打印守护程序），它不支持IPP（Internet打印协议），而且也不支持同时使用多个打印设备。

CUPS给Unix/Linux用户提供了一种可靠有效的方法来管理打印。它支持IPP，并提供了LPD，SMB（服务消息块，如配置为微软WINDOWS的打印机）、JetDirect等接口。CUPS还可以浏览网络打印机。[1] 
作为网络服务器建议关闭CUPS，关闭CUPS的命令如下：
```shell
service cups stop

chkconfig cups off
```
## Debian环境下安装CUPS

关于如何安装，网上已经有非常完整的教程了，这里就不再赘述。

```shell
sudo apt-get install cups
```

更多信息可以点击[这里](http://www.penguintutor.com/linux/printing-cups)

## 安装pycups
其实这里才是我想说的重点，因为确实是在这里遇到了问题。前面相当于安装好了程序，而pycups则是CUPS服务的python接口，就是可以用python来控制CUPS。

1. 下载pycups源码

```shell
git clone git://git.fedorahosted.org/git/pycups.git
```

下载完成之后，进入目录，正常情况下应该进行安装了。

2. 安装pycups

当导航至文件夹pycups之后，执行安装命令。

```shell
sudo python setup.py install
```

正常的情况下就会安装成功，不过却出现了几个问题。

**问题1**

![](https://raw.githubusercontent.com/Lee-Kevin/images/master/2016/2016-10-11/1.jpg)

提示无法找到Python.h文件，其实出现这种问题是因为没有安装python开发包。

解决方法：

```shell
sudo apt-get install python-dev
```

**问题2**

问题1解决之后，继续执行安装命令，发现有出现了新的问题。

![](https://raw.githubusercontent.com/Lee-Kevin/images/master/2016/2016-10-11/2.jpg)

提示无法找到cups.h文件，这个问题跟上一个类似，没有安装CUPS的开发包。

解决方法：

```shell
sudo apt-get install libcups2-dev
```

这时再次执行安装命令，就会发现pycups可以正常安装了。

## 测试pycups
进入python环境，进行如下测试。

```python
>>> # Example of getting a list of printers
>>> import cups
>>> conn = cups.Connection ()
>>> printers = conn.getPrinters ()
>>> for printer in printers:
...     print printer, printers[printer]["device-uri"]
...
HP ipp://192.168.1.1:631/printers/HP
duplex ipp://192.168.1.1:631/printers/duplex
HP-LaserJet-6MP ipp://192.168.1.1:631/printers/HP-LaserJet-6MP
EPSON-Stylus-D78 usb://EPSON/Stylus%20D78
```
## 总结
知识点很简单，但还是写下来了，一是逼迫自己养成写技术博客的习惯，二是确实自己在linux下调试安装的经验缺乏，需要多多积累。

后续项目完成之后，还会继续分享pycups的使用。

## 相关资料

- [https://pypi.python.org/pypi/pycups](https://pypi.python.org/pypi/pycups)
- [http://cyberelk.net/tim/software/pycups/](http://cyberelk.net/tim/software/pycups/)
- [http://www.penguintutor.com/linux/printing-cups](http://www.penguintutor.com/linux/printing-cups)