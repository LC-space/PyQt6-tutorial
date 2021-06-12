[目录]() [下一章]()

# 介绍PyQt6

*最近修改于2021年4月22日*

这是PyQt6的介绍性教程。本教程的目的是让您开始使用PyQt6工具包。

## 关于PyQt6

pyqt6是一套Python绑定Digia QT5应用的框架。Qt库是最强大的GUI库之一。PyQt6的官方主页是https://www.riverbankcomputing.co.uk/news。PyQt6是由Riverbank Computing公司开发的。

pyqt6是Python的一个模块。它是一个多平台的工具包，可以在所有主要的操作系统上运行，包括Unix、Windows和Mac OS。PyQt6是双重许可；开发人员可以在GPL和商业许可之间进行选择。

## PyQt6模块

PyQt6的类被分为几个模块，包括以下模块：

- QtCore
- QtGui
- QtWidgets
- QtDBus
- QtNetwork
- QtHelp
- QtXml
- QtSvg
- QtSql
- QtTest

QtCore模块包含了核心的非GUI功能。这个模块用于处理时间、文件和目录、各种数据类型、流、URL、MIME类型、线程或进程。QtGui包含窗口系统集成、事件处理、2D图形、基本图像、字体和文本等类。QtWidgets模块包含的类提供了一组UI元素来创建经典的桌面风格的用户界面。

QtDBus包含使用D-Bus协议支持IPC的类。QtNetwork模块包含用于网络编程的类。这些类通过使网络编程更简单、更易移植来方便TCP/IP和UDP客户端和服务器的编码。QtHelp包含用于创建和查看可搜索文档的类。

QtXml包含用于处理XML文件的类。这个模块提供了SAX和DOM api的实现。QtSvg模块提供了显示SVG文件内容的类。可伸缩矢量图形(SVG)是一种用XML描述二维图形和图形应用程序的语言。QtSql模块提供了处理数据库的类。QtTest包含启用PyQt6应用程序单元测试的函数。

## Python

Python是一种通用的、动态的、面向对象的编程语言。Python语言的设计目的强调程序员的工作效率和代码的可读性。它于1991年首次发布。Python受到ABC、Haskell、Java、Lisp、Icon和Perl编程语言的启发。Python是一种高级的、通用的、多平台的解释语言。Python是一种极简语言。Python是由世界各地的一群志愿者维护的。

Python编程语言的官方网站是[python.org](https://python.org/)。

## PyQt6版本

QT_VERSION_STR提供了Qt的版本和PyQt6的PYQT_VERSION_STR版本。

```Python
# version.py
#!/usr/bin/python

from PyQt6.QtCore import QT_VERSION_STR
from PyQt6.QtCore import PYQT_VERSION_STR

print(QT_VERSION_STR)
print(PYQT_VERSION_STR)
```

我们打印Qt库和PyQt6模块的版本。

```
6.1.0
6.1.0
```

本章介绍了PyQt6工具包。

