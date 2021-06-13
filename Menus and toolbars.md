[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章]() [下一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/First%20programs.md)

[TOC]

# PyQt6中的菜单和工具栏

*最近更新于2021年4月24日*

在PyQt6教程的这一部分中，我们将创建状态栏、菜单栏和工具栏。菜单是位于菜单栏中的一组命令。工具栏具有应用程序中一些常用命令的按钮。状态栏显示状态信息，通常位于应用程序窗口的底部。

## PyQt6 QMainWindow

QMainWindow类提供了一个主应用程序窗口。这样就可以创建一个带有状态栏、工具栏和菜单栏的经典应用程序框架。

## PyQt6状态栏

状态栏是用于显示状态信息的控件。

```python
# statusbar.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a statusbar.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QApplication


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.statusBar().showMessage('Ready')

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Statusbar')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

状态栏是在QMainWindow控件的帮助下创建的。

```python
self.statusBar().showMessage('Ready')
```

为了获得状态栏，我们调用QtGui.QMainWindow类的statusBar方法。该方法的第一次调用将创建一个状态栏。随后的调用返回状态栏对象。showMessage在状态栏上显示一条消息。

## PyQt6简单的菜单

菜单栏是GUI应用程序的常见部分。它是位于各种菜单中的一组命令。(Mac OS对菜单的处理方式不同。要获得类似的结果，我们可以添加以下行:menubar.setNativeMenuBar(False)。

```python
# simple_menu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a menubar. The
menubar has one menu with an exit action.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QApplication
from PyQt6.QtGui import QIcon, QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        exitAct = QAction(QIcon('https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/exit.png'), '&Exit', self)
        exitAct.setShortcut('Ctrl+Q')
        exitAct.setStatusTip('Exit application')
        exitAct.triggered.connect(QApplication.instance().quit)

        self.statusBar()

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Simple menu')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在上面的例子中，我们创建了一个只有一个菜单的菜单栏。此菜单包含一个操作，如果选中，该操作将终止应用程序。状态栏也会被创建。该操作可以通过Ctrl+Q快捷键访问。

```python
exitAct = QAction(QIcon('https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/exit.png'), '&Exit', self)
exitAct.setShortcut('Ctrl+Q')
exitAct.setStatusTip('Exit application')
```

QAction是对使用菜单栏、工具栏或自定义键盘快捷键执行的操作的抽象。在上面的三行代码中，我们创建了一个带有特定图标和'Exit'标签的操作。此外，还为该操作定义了一个快捷方式。第三行创建一个状态提示，当我们将鼠标指针悬停在菜单项上时，它将显示在状态栏中。

```python
exitAct.triggered.connect(QApplication.instance().quit)
```

当我们选择这个特定的动作时，一个被触发的信号被发射。信号连接到QApplication控件的退出方法。这将终止应用程序。

```python
menubar = self.menuBar()
fileMenu = menubar.addMenu('&File')
fileMenu.addAction(exitAct)
```

menuBar方法创建一个菜单栏。我们用addMenu创建一个文件菜单，并用addAction添加动作。

## PyQt6子菜单

子菜单是位于另一个菜单中的菜单。

```python
# submenu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a submenu.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QMenu, QApplication
from PyQt6.QtGui import QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('File')

        impMenu = QMenu('Import', self)
        impAct = QAction('Import mail', self)
        impMenu.addAction(impAct)

        newAct = QAction('New', self)

        fileMenu.addAction(newAct)
        fileMenu.addMenu(impMenu)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Submenu')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在这个例子中，我们有两个菜单项;一个位于文件菜单，另一个位于文件的导入子菜单。

```python
impMenu = QMenu('Import', self)
```

使用QMenu创建新菜单。

```python
impAct = QAction('Import mail', self)
impMenu.addAction(impAct)
```

通过addAction向子菜单添加一个操作。

![image-20210613210325588](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/image-20210613210325588.png)

## PyQt6检查菜单

在下面的示例中，我们创建了一个可以勾选和取消勾选的菜单。

```python
# check_menu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a checkable menu.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QApplication
from PyQt6.QtGui import QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.statusbar = self.statusBar()
        self.statusbar.showMessage('Ready')

        menubar = self.menuBar()
        viewMenu = menubar.addMenu('View')

        viewStatAct = QAction('View statusbar', self, checkable=True)
        viewStatAct.setStatusTip('View statusbar')
        viewStatAct.setChecked(True)
        viewStatAct.triggered.connect(self.toggleMenu)

        viewMenu.addAction(viewStatAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Check menu')
        self.show()


    def toggleMenu(self, state):

        if state:
            self.statusbar.show()
        else:
            self.statusbar.hide()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

代码示例创建了一个带有一个操作的View菜单。该操作显示或隐藏状态栏。当状态栏可见时，菜单项被选中。

```python
viewStatAct = QAction('View statusbar', self, checkable=True)
```

使用可勾选选项，我们创建了一个可勾选菜单。

```
viewStatAct.setChecked(True)
```

因为状态栏从一开始就是可见的，所以我们用setChecked方法来勾选这个动作。

```python
def toggleMenu(self, state):

    if state:
        self.statusbar.show()
    else:
        self.statusbar.hide()
```

根据操作的状态，显示或隐藏状态栏。

![image-20210613211050889](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/image-20210613211050889.png)

## PyQt6上下文菜单

上下文菜单，也称为弹出菜单，是出现在某个上下文下的命令列表。例如，在Opera网络浏览器中，当我们右击网页时，我们会得到一个上下文菜单。在这里，我们可以重新加载页面、返回或查看页面源。如果我们右键单击工具栏，我们会得到另一个管理工具栏的上下文菜单。

```python
# context_menu.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a context menu.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QMenu, QApplication


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Context menu')
        self.show()


    def contextMenuEvent(self, event):

        cmenu = QMenu(self)

        newAct = cmenu.addAction("New")
        openAct = cmenu.addAction("Open")
        quitAct = cmenu.addAction("Quit")
        action = cmenu.exec(self.mapToGlobal(event.pos()))

        if action == quitAct:
            QApplication.instance().quit()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

要使用上下文菜单，我们必须重新实现contextMenuEvent方法。

```python
action = cmenu.exec(self.mapToGlobal(event.pos()))
```

使用exec方法显示上下文菜单。从事件对象获取鼠标指针的坐标。mapToGlobal方法将控件坐标转换为全局屏幕坐标。

```python
if action == quitAct:
    QApplication.instance().quit()
```

如果从上下文菜单返回的操作等于quit操作，则终止应用程序。

## PyQt6工具栏

菜单将我们可以在应用程序中使用的所有命令分组。工具栏提供了对最常用命令的快速访问。

```python
# toolbar.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a toolbar.
The toolbar has one action, which
terminates the application, if triggered.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow,  QApplication
from PyQt6.QtGui import QIcon, QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        exitAct = QAction(QIcon('https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/exit.png'), 'Exit', self)
        exitAct.setShortcut('Ctrl+Q')
        exitAct.triggered.connect(QApplication.instance().quit)

        self.toolbar = self.addToolBar('Exit')
        self.toolbar.addAction(exitAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Toolbar')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在上面的示例中，我们创建了一个简单的工具栏。工具栏有一个工具操作，即当被触发时终止应用程序的退出操作。

```python
exitAct = QAction(QIcon('https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/exit.png'), 'Exit', self)
exitAct.setShortcut('Ctrl+Q')
exitAct.triggered.connect(QApplication.instance().quit)
```

类似于上面的菜单栏示例，我们创建了一个动作对象。该对象具有标签、图标和快捷方式。QApplication的退出方法连接到触发信号。

```python
self.toolbar = self.addToolBar('Exit')
self.toolbar.addAction(exitAction)
```

工具栏是使用addToolBar方法创建的。我们通过addAction向工具栏添加一个动作对象。

## PyQt6主窗口

在本节的最后一个示例中，我们将创建一个菜单栏、工具栏和状态栏。我们还创建了一个中心控件。

```python
# main_window.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a skeleton of
a classic GUI application with a menubar,
toolbar, statusbar, and a central widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QTextEdit, QApplication
from PyQt6.QtGui import QIcon, QAction


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        textEdit = QTextEdit()
        self.setCentralWidget(textEdit)

        exitAct = QAction(QIcon('https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/exit.png'), 'Exit', self)
        exitAct.setShortcut('Ctrl+Q')
        exitAct.setStatusTip('Exit application')
        exitAct.triggered.connect(self.close)

        self.statusBar()

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(exitAct)

        toolbar = self.addToolBar('Exit')
        toolbar.addAction(exitAct)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Main window')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

这个代码示例创建了一个带有菜单栏、工具栏和状态栏的经典GUI应用程序的框架。

```python
textEdit = QTextEdit()
self.setCentralWidget(textEdit)
```

这里我们创建了一个文本编辑控件。我们将它设置为QMainWindow的中心小部件。中央小部件会占用所有剩余的空间。

![image-20210613214024743](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Menus%20and%20toolbars/img/image-20210613214024743.png)

在PyQt6教程的这一部分中，我们使用了菜单、工具栏、状态栏和主应用程序窗口。