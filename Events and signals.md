[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章]() [下一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Layout%20management.md)

# PyQt6事件和信号

*最近更新于2021年4月29日*

在PyQt6编程教程的这一部分中，我们将探索应用程序中发生的事件和信号。

## PyQt6事件

GUI应用程序是事件驱动的。事件主要由应用程序的用户生成。但它们也可以通过其他方式产生；例如，一个互联网连接，一个窗口管理器，或一个定时器。当我们调用应用程序的exec()方法时，应用程序进入主循环。主循环获取事件并将它们发送给对象。

在事件模型中，有三个参与者:

* 事件源
* 事件对象
* 事件目标

事件源是状态发生变化的对象。它生成事件。事件对象（event）封装了事件源中的状态更改。事件目标是希望被通知的对象。事件源对象将处理事件的任务委托给事件目标。

## PyQt6信号和槽

这是一个简单的例子，演示了PyQt6中的信号和槽。

```python
# signals_slots.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we connect a signal
of a QSlider to a slot of a QLCDNumber.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtCore import Qt
from PyQt6.QtWidgets import (QWidget, QLCDNumber, QSlider,
        QVBoxLayout, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        lcd = QLCDNumber(self)
        sld = QSlider(Qt.Orientation.Horizontal, self)

        vbox = QVBoxLayout()
        vbox.addWidget(lcd)
        vbox.addWidget(sld)

        self.setLayout(vbox)
        sld.valueChanged.connect(lcd.display)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Signal and slot')
        self.show()


def main():
    
    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的例子中，我们显示了一个QtGui。qcdnumber和QtGui.QSlider。我们通过拖动滑块旋钮来改变液晶显示数字。

```python
sld.valueChanged.connect(lcd.display)
```

这里我们将滑块的valueChanged信号连接到lcd数字的显示槽。

发送方是一个发送信号的对象。接收器是接收信号的对象。槽是对信号作出反应的方法。

![image-20210614204459852](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Events%20and%20signals/img/image-20210614204459852.png)

## PyQt6重新实现事件处理程序

PyQt6中的事件通常通过重新实现事件处理程序来处理。

```python
# reimplement_handler.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we reimplement an
event handler.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtCore import Qt
from PyQt6.QtWidgets import QWidget, QApplication


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Event handler')
        self.show()


    def keyPressEvent(self, e):
        
        if e.key() == Qt.Key.Key_Escape.value:
            self.close()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们重新实现了keyPressEvent事件处理程序。

```python
def keyPressEvent(self, e):
    
    if e.key() == Qt.Key.Key_Escape.value:
        self.close()
```

如果单击Escape按钮，应用程序将终止。

## PyQt6事件对象

事件对象是一个Python对象，它包含许多描述事件的属性。事件对象特定于生成的事件类型。

```python
# event_object.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we display the x and y
coordinates of a mouse pointer in a label widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtCore import Qt
from PyQt6.QtWidgets import QWidget, QApplication, QGridLayout, QLabel


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        grid = QGridLayout()

        x = 0
        y = 0

        self.text = f'x: {x},  y: {y}'

        self.label = QLabel(self.text, self)
        grid.addWidget(self.label, 0, 0, Qt.AlignmentFlag.AlignTop)

        self.setMouseTracking(True)
        self.setLayout(grid)

        self.setGeometry(300, 300, 450, 300)
        self.setWindowTitle('Event object')
        self.show()


    def mouseMoveEvent(self, e):

        x = int(e.position().x())
        y = int(e.position().y())

        text = f'x: {x},  y: {y}'
        self.label.setText(text)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在本例中，我们在标签控件中显示鼠标指针的x和y坐标。

```python
self.setMouseTracking(True)
```

鼠标跟踪在默认情况下是禁用的，所以当鼠标被移动时至少有一个鼠标按钮被按下时，控件才接收鼠标移动事件。如果启用了鼠标跟踪，那么即使没有按下按钮，控件也会接收鼠标移动事件。

```python
def mouseMoveEvent(self, e):

    x = int(e.position().x())
    y = int(e.position().y())
    ...
```

e是事件对象；它包含有关被触发事件的数据。在我们的例子中，它是鼠标移动事件。通过position().x()和e.p eposition ().y()方法，我们确定了鼠标指针的x和y坐标。

```python
self.text = f'x: {x},  y: {y}'
self.label = QLabel(self.text, self)
```

x和y坐标显示在QLabel控件中。

![image-20210614210417063](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Events%20and%20signals/img/image-20210614210417063.png)

## PyQt6事件发送方

有时，知道哪个控件是信号的发送者是很方便的。为此，PyQt6有sender方法。

```python
# event_sender.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we determine the event sender
object.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QMainWindow, QPushButton, QApplication


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        btn1 = QPushButton("Button 1", self)
        btn1.move(30, 50)

        btn2 = QPushButton("Button 2", self)
        btn2.move(150, 50)

        btn1.clicked.connect(self.buttonClicked)
        btn2.clicked.connect(self.buttonClicked)

        self.statusBar()

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Event sender')
        self.show()


    def buttonClicked(self):

        sender = self.sender()

        msg = f'{sender.text()} was pressed'
        self.statusBar().showMessage(msg)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中有两个按钮。在buttonclick方法中，我们通过调用发送方方法来确定我们点击了哪个按钮。

```python
btn1.clicked.connect(self.buttonClicked)
btn2.clicked.connect(self.buttonClicked)
```

两个按钮连接在同一个槽位上。

```python
def buttonClicked(self):

    sender = self.sender()

    msg = f'{sender.text()} was pressed'
    self.statusBar().showMessage(msg)
```

我们通过调用发送方方法来确定信号源。在应用程序的状态栏中，我们显示了被按下的按钮的标签。

![image-20210614212436159](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Events%20and%20signals/img/image-20210614212436159.png)

## PyQt6发出信号

从QObject创建的对象可以发出信号。下面的示例展示了如何发出自定义信号。

```python
# custom_signal.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we show how to
emit a custom signal.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtCore import pyqtSignal, QObject
from PyQt6.QtWidgets import QMainWindow, QApplication


class Communicate(QObject):

    closeApp = pyqtSignal()


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.c = Communicate()
        self.c.closeApp.connect(self.close)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Emit signal')
        self.show()


    def mousePressEvent(self, e):

        self.c.closeApp.emit()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

我们创建一个名为closeApp的新信号。这个信号在鼠标按下事件期间发出。信号连接到qmain窗口的关闭槽。

```python
class Communicate(QObject):

    closeApp = pyqtSignal()
```

使用pyqtSignal作为外部通信类的类属性创建信号。

```python
self.c = Communicate()
self.c.closeApp.connect(self.close)
```

自定义closeApp信号连接到QMainWindow的关闭槽。

```python
def mousePressEvent(self, event):

    self.c.closeApp.emit()
```

当我们用鼠标指针单击窗口时，会发出closeApp信号。应用程序终止。

在PyQt6教程的这一部分中，我们讨论了信号和槽。

