[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Widgets%20II.md) [下一章]()

# PyQt6中的拖放

*最近更新于2021年5月15日*

在PyQt6教程的这一部分中，我们将介绍拖放操作。

在计算机图形用户界面中，拖放是点击一个虚拟对象并将其拖到不同位置或另一个虚拟对象的动作(或支持该动作)。一般来说，它可以用来调用多种类型的操作，或者在两个抽象对象之间创建各种类型的关联。

拖放是图形用户界面的一部分。拖放操作可以让用户直观地做复杂的事情。	

通常，我们可以拖放两个东西：数据或一些图形对象。如果我们将图像从一个应用程序拖到另一个应用程序，我们就拖放二进制数据。如果我们在Firefox中拖动一个标签并将其移动到另一个地方，我们就拖放了一个图形组件。

## QDrag

QDrag支持基于MIME的拖放数据传输。它处理拖放操作的大部分细节。传输的数据包含在QMimeData对象中。

## PyQt6中的简单拖放示例

在第一个例子中，我们有一个QLineEdit和一个QPushButton。我们从line edit控件中拖出纯文本，并将其拖放到按钮控件上。按钮的标签将改变。

```python
# simple.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This is a simple drag and
drop example.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtWidgets import (QPushButton, QWidget,
        QLineEdit, QApplication)


class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)

        self.setAcceptDrops(True)


    def dragEnterEvent(self, e):

        if e.mimeData().hasFormat('text/plain'):
            e.accept()
        else:
            e.ignore()


    def dropEvent(self, e):

        self.setText(e.mimeData().text())


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        edit = QLineEdit('', self)
        edit.setDragEnabled(True)
        edit.move(30, 65)

        button = Button("Button", self)
        button.move(190, 65)

        self.setWindowTitle('Simple drag and drop')
        self.setGeometry(300, 300, 300, 150)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    ex.show()
    app.exec()


if __name__ == '__main__':
    main()
```

这个例子展示了一个简单的拖放操作。

```python
class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)

        ...
```

为了在QPushButton控件上拖放文本，我们必须重新实现一些方法。因此，我们创建了自己的Button类，它继承自QPushButton类。

```python
self.setAcceptDrops(True)
```

我们使用setAcceptDrops为控件启用拖放事件。

```python
def dragEnterEvent(self, e):

    if e.mimeData().hasFormat('text/plain'):
        e.accept()
    else:
        e.ignore()
```

首先，我们重新实现dragEnterEvent方法。我们通知我们接受的数据类型。在我们的例子中，它是纯文本。

```python
def dropEvent(self, e):

    self.setText(e.mimeData().text())
```

通过重新实现dropEvent方法，我们定义了在删除事件中发生的事情。这里我们更改按钮控件的文本。

```python
edit = QLineEdit('', self)
edit.setDragEnabled(True)
```

QLineEdit控件内置了对拖动操作的支持。我们需要做的就是调用setDragEnabled方法来激活它。

![image-20210618151201975](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Drag%20%26%20drop/img/image-20210618151201975.png)

## 拖放一个按钮控件

下面的示例演示了如何拖放按钮控件。

```python
# drag_button.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this program, we can press on a button with a left mouse
click or drag and drop the button with  the right mouse click.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtCore import Qt, QMimeData
from PyQt6.QtGui import QDrag
from PyQt6.QtWidgets import QPushButton, QWidget, QApplication


class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)


    def mouseMoveEvent(self, e):

        if e.buttons() != Qt.MouseButton.RightButton:
            return

        mimeData = QMimeData()

        drag = QDrag(self)
        drag.setMimeData(mimeData)

        drag.setHotSpot(e.position().toPoint() - self.rect().topLeft())

        dropAction = drag.exec(Qt.DropAction.MoveAction)


    def mousePressEvent(self, e):

        super().mousePressEvent(e)

        if e.button() == Qt.MouseButton.LeftButton:
            print('press')


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setAcceptDrops(True)

        self.button = Button('Button', self)
        self.button.move(100, 65)

        self.setWindowTitle('Click or Move')
        self.setGeometry(300, 300, 550, 450)


    def dragEnterEvent(self, e):

        e.accept()


    def dropEvent(self, e):

        position = e.position()
        self.button.move(position.toPoint())

        e.setDropAction(Qt.DropAction.MoveAction)
        e.accept()


def main():
    
    app = QApplication(sys.argv)
    ex = Example()
    ex.show()
    app.exec()


if __name__ == '__main__':
    main()
```

在我们的代码示例中，窗口上有一个QPushButton。如果我们用鼠标左键点击按钮，‘press’信息就会打印到控制台。通过右键单击并移动按钮，我们将对按钮控件执行拖放操作。

```python
class Button(QPushButton):

    def __init__(self, title, parent):
        super().__init__(title, parent)
```

我们创建了一个来自QPushButton的Button类。我们还重新实现了QPushButton的两个方法:mouseemoveevent和mousePressEvent。mouseMoveEvent方法是拖放操作开始的地方。

```python
if e.buttons() != Qt.MouseButton.RightButton:
    return
```

这里我们决定只使用鼠标右键来执行拖放操作。鼠标左键保留用于单击该按钮。

```python
drag = QDrag(self)
drag.setMimeData(mimeData)

drag.setHotSpot(e.position().toPoint() - self.rect().topLeft())
```

QDrag对象被创建。该类提供了对基于MIME的拖放数据传输的支持。

```python
dropAction = drag.exec(Qt.DropAction.MoveAction)
```

拖动对象的exec方法将启动拖放操作。

```python
def mousePressEvent(self, e):

    super().mousePressEvent(e)

    if e.button() == Qt.MouseButton.LeftButton:
        print('press')
```

如果我们用鼠标左键点击按钮，我们将“press”打印到控制台。注意，我们也在父类上调用mousePressEvent方法。否则，我们将看不到按钮被按下。

```python
position = e.pos()
self.button.move(position)
```

在dropEvent方法中，我们指定释放鼠标按钮并完成放操作后发生的事情。在我们的例子中，我们找到当前鼠标指针的位置，并相应地移动按钮。

```python
e.setDropAction(Qt.MoveAction)
e.accept()
```

我们使用setDropAction来指定拖放操作的类型。在我们的例子中，它是一个移动动作。

PyQt6教程的这一部分专门用于拖放操作。

[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Widgets%20II.md) [下一章]()

