[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Dialogs.md) [下一章]()

# PyQt6控件

*最近更新于2021年5月3日*

控件是应用程序的基本构建块。PyQt6有各种各样的控件，包括按钮、复选框、滑块或列表框。在本教程的这一节中，我们将描述几个有用的小部件:QCheckBox、toggle模式下的QPushButton、QSlider、QProgressBar和QCalendarWidget。

## PyQt6 QCheckBox

QCheckBox是一个具有两种状态的控件：开和关。这是一个带标签的盒子。复选框通常用于表示应用程序中可以启用或禁用的特性。

```python
# check_box.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, a QCheckBox widget
is used to toggle the title of a window.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import QWidget, QCheckBox, QApplication
from PyQt6.QtCore import Qt
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        cb = QCheckBox('Show title', self)
        cb.move(20, 20)
        cb.toggle()
        cb.stateChanged.connect(self.changeTitle)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('QCheckBox')
        self.show()


    def changeTitle(self, state):

        if state == Qt.CheckState.Checked.value:
            self.setWindowTitle('QCheckBox')
        else:
            self.setWindowTitle(' ')


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

我们创建一个用于切换窗口标题的复选框。

```python
cb = QCheckBox('Show title', self)
```

这是一个QCheckBox构造函数。

```python
cb.toggle()
```

我们已经设置了窗口标题，所以我们也选中了复选框。

```python
cb.stateChanged.connect(self.changeTitle)
```

我们将用户定义的changeTitle方法连接到stateChanged信号。changeTitle方法切换窗口标题。

```python
if state == Qt.CheckState.Checked.value:
    self.setWindowTitle('QCheckBox')
else:
    self.setWindowTitle(' ')
```

控件的状态被赋给状态变量中的changeTitle方法。如果控件被选中，我们将设置窗口的标题。否则，我们为标题栏设置一个空字符串。

![image-20210617204451891](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets/img/image-20210617204451891.png)

## 开关按钮

开关按钮是一个特殊模式下的QPushButton。这个按钮有两种状态：按下和未按下。我们通过点击它来在这两种状态之间切换。

```python
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we create three toggle buttons.
They control the background color of a QFrame.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QPushButton,
        QFrame, QApplication)
from PyQt6.QtGui import QColor
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.col = QColor(0, 0, 0)

        redb = QPushButton('Red', self)
        redb.setCheckable(True)
        redb.move(10, 10)

        redb.clicked[bool].connect(self.setColor)

        greenb = QPushButton('Green', self)
        greenb.setCheckable(True)
        greenb.move(10, 60)

        greenb.clicked[bool].connect(self.setColor)

        blueb = QPushButton('Blue', self)
        blueb.setCheckable(True)
        blueb.move(10, 110)

        blueb.clicked[bool].connect(self.setColor)

        self.square = QFrame(self)
        self.square.setGeometry(150, 20, 100, 100)
        self.square.setStyleSheet("QWidget { background-color: %s }" %
                                  self.col.name())

        self.setGeometry(300, 300, 300, 250)
        self.setWindowTitle('Toggle button')
        self.show()


    def setColor(self, pressed):

        source = self.sender()

        if pressed:
            val = 255
        else:
            val = 0

        if source.text() == "Red":
            self.col.setRed(val)
        elif source.text() == "Green":
            self.col.setGreen(val)
        else:
            self.col.setBlue(val)

        self.square.setStyleSheet("QFrame { background-color: %s }" %
                                  self.col.name())


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们创建了三个开关按钮和一个QWidget。我们将QWidget的背景颜色设置为黑色。切换按钮切换颜色值的红色、绿色和蓝色部分。背景颜色取决于所按的切换按钮。

```python
self.col = QColor(0, 0, 0)
```

这是初始的黑色值。

```python
redb = QPushButton('Red', self)
redb.setCheckable(True)
redb.move(10, 10)
```

要创建一个开关按钮，我们创建一个QPushButton，并通过调用setCheckable方法使其可选择。

```python
redb.clicked[bool].connect(self.setColor)
```

我们将一个点击信号连接到用户定义的方法。我们使用使用布尔值的clicked信号。

```python
source = self.sender()
```

我们得到了被切换的按钮。

```python
if source.text() == "Red":
    self.col.setRed(val)
```

如果是红色按钮，我们会相应地更新颜色的红色部分。

```python
self.square.setStyleSheet("QFrame { background-color: %s }" %
    self.col.name())
```

我们使用样式表来改变背景颜色。使用setStyleSheet方法更新样式表。

![image-20210617213535523](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets/img/image-20210617213535523.png)

## PyQt6 QSlider

QSlider是一个具有简单滑块的控件。这个滑块可以左右拉动。通过这种方式，我们可以为特定的任务选择一个值。有时使用滑块比输入数字或使用旋转框更自然。

在我们的例子中，我们显示了一个滑块和一个标签。标签显示图像。滑块控制标签。

```python
# slider.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows a QSlider widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QSlider,
        QLabel, QApplication)
from PyQt6.QtCore import Qt
from PyQt6.QtGui import QPixmap
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        sld = QSlider(Qt.Orientation.Horizontal, self)
        sld.setFocusPolicy(Qt.FocusPolicy.NoFocus)
        sld.setGeometry(30, 40, 200, 30)
        sld.valueChanged[int].connect(self.changeValue)

        self.label = QLabel(self)
        self.label.setPixmap(QPixmap('img/mute.png'))
        self.label.setGeometry(250, 40, 80, 30)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('QSlider')
        self.show()


    def changeValue(self, value):

        if value == 0:

            self.label.setPixmap(QPixmap('img/mute.png'))
        elif 0 < value <= 30:

            self.label.setPixmap(QPixmap('img/min.png'))
        elif 30 < value < 80:

            self.label.setPixmap(QPixmap('img/med.png'))
        else:

            self.label.setPixmap(QPixmap('img/max.png'))


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们模拟音量控制。通过拖动滑块的手柄，我们可以改变标签上的图像。

```python
sld = QSlider(Qt.Orientation.Horizontal, self)
```

这里我们创建了一个水平的QSlider。

```python
self.label = QLabel(self)
self.label.setPixmap(QPixmap('img/mute.png'))
```

我们创建一个QLabel小部件，并为它设置一个初始的静音图像。

```python
sld.valueChanged[int].connect(self.changeValue)
```

我们将valueChanged信号连接到用户定义的changeValue方法。

```python
if value == 0:
    self.label.setPixmap(QPixmap('img/mute.png'))
...
```

根据滑块的值，我们将图像设置为标签。在上面的代码中，如果滑块等于0，我们将mute .png图像设置到标签。

![image-20210617223034230](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets/img/image-20210617223034230.png)

## PyQt6 QProgressBar

进度条是处理冗长任务时使用的控件。它是动画的，以便用户知道任务正在进行。QProgressBar控件在PyQt6工具箱中提供了一个水平或垂直的进度条。程序员可以为进度条设置最小值和最大值。默认值为0和99。

```python
# progressbar.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows a QProgressBar widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QProgressBar,
        QPushButton, QApplication)
from PyQt6.QtCore import QBasicTimer
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.pbar = QProgressBar(self)
        self.pbar.setGeometry(30, 40, 200, 25)

        self.btn = QPushButton('Start', self)
        self.btn.move(40, 80)
        self.btn.clicked.connect(self.doAction)

        self.timer = QBasicTimer()
        self.step = 0

        self.setGeometry(300, 300, 280, 170)
        self.setWindowTitle('QProgressBar')
        self.show()


    def timerEvent(self, e):

        if self.step >= 100:

            self.timer.stop()
            self.btn.setText('Finished')
            return

        self.step = self.step + 1
        self.pbar.setValue(self.step)


    def doAction(self):

        if self.timer.isActive():
            self.timer.stop()
            self.btn.setText('Start')
        else:
            self.timer.start(100, self)
            self.btn.setText('Stop')


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的例子中，我们有一个水平进度条和一个按钮。按钮启动和停止进度条。

```python
self.pbar = QProgressBar(self)
```

这是一个QProgressBar构造函数。

```python
self.timer = QBasicTimer()
```

为了激活进度条，我们使用一个计时器对象。

```python
self.timer.start(100, self)
```

要启动一个计时器事件，我们调用它的start方法。这个方法有两个参数：超时和接收事件的对象。

```python
def timerEvent(self, e):

    if self.step >= 100:

        self.timer.stop()
        self.btn.setText('Finished')
        return

    self.step = self.step + 1
    self.pbar.setValue(self.step)
```

每个QObject及其后代都有一个timerEvent事件处理程序。为了响应计时器事件，我们重新实现了事件处理程序。

```python
def doAction(self):

    if self.timer.isActive():
        self.timer.stop()
        self.btn.setText('Start')

    else:
        self.timer.start(100, self)
        self.btn.setText('Stop')
```

在doAction方法中，我们启动和停止计时器。

![image-20210617223524029](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets/img/image-20210617223524029.png)

## PyQt6 QCalendarWidget

QCalendarWidget提供了一个基于月度的日历控件。它允许用户以简单直观的方式选择日期。

```python
# calendar.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows a QCalendarWidget widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QCalendarWidget,
        QLabel, QApplication, QVBoxLayout)
from PyQt6.QtCore import QDate
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        vbox = QVBoxLayout(self)

        cal = QCalendarWidget(self)
        cal.setGridVisible(True)
        cal.clicked[QDate].connect(self.showDate)

        vbox.addWidget(cal)

        self.lbl = QLabel(self)
        date = cal.selectedDate()
        self.lbl.setText(date.toString())

        vbox.addWidget(self.lbl)

        self.setLayout(vbox)

        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Calendar')
        self.show()


    def showDate(self, date):
        self.lbl.setText(date.toString())


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

这个示例有一个日历控件和一个标签控件。当前选择的日期显示在标签控件中。

```python
cal = QCalendarWidget(self)
```

QCalendarWidget被创建。

```python
cal.clicked[QDate].connect(self.showDate)
```

如果我们从小部件中选择一个日期，则会发出一个单击的[QDate]信号。我们将这个信号连接到用户定义的showDate方法。

```python
def showDate(self, date):

    self.lbl.setText(date.toString())
```

我们通过调用selectedDate方法来检索选定的日期。然后我们将日期对象转换为字符串，并将其设置为标签控件。

在PyQt6教程的本部分中，我们已经介绍了以下控件：QCheckBox、在tooggle模式下的QPushButton、QSlider、QProgressBar和QCalendarWidget。

[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Dialogs.md) [下一章]()