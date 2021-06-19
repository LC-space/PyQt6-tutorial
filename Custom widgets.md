[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Painting.md) [下一章]()

# PyQt6中的自定义控件

*最近更新于2021年5月17日*

PyQt6有一组丰富的控件。但是，没有工具箱能够提供程序员在其应用程序中可能需要的所有控件。工具箱通常只提供最常见的控件，如按钮、文本控件或滑块。如果需要一个更专业的控件，我们必须自己创建它。

使用工具箱提供的绘图工具创建自定义控件。有两种基本的可能性：程序员可以修改或增强现有的控件，也可以从头创建自定义控件。

## PyQt6烧录控件

这是我们可以在Nero、K3B或其他CD/DVD刻录软件中看到的一个控件。

```python
# burning_widget.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we create a custom widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QSlider, QApplication,
        QHBoxLayout, QVBoxLayout)
from PyQt6.QtCore import QObject, Qt, pyqtSignal
from PyQt6.QtGui import QPainter, QFont, QColor, QPen
import sys


class Communicate(QObject):
    updateBW = pyqtSignal(int)


class BurningWidget(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setMinimumSize(1, 30)
        self.value = 75
        self.num = [75, 150, 225, 300, 375, 450, 525, 600, 675]


    def setValue(self, value):

        self.value = value


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        self.drawWidget(qp)
        qp.end()


    def drawWidget(self, qp):

        MAX_CAPACITY = 700
        OVER_CAPACITY = 750

        font = QFont('Serif', 7, QFont.Weight.Light)
        qp.setFont(font)

        size = self.size()
        w = size.width()
        h = size.height()

        step = int(round(w / 10))

        till = int(((w / OVER_CAPACITY) * self.value))
        full = int(((w / OVER_CAPACITY) * MAX_CAPACITY))

        if self.value >= MAX_CAPACITY:

            qp.setPen(QColor(255, 255, 255))
            qp.setBrush(QColor(255, 255, 184))
            qp.drawRect(0, 0, full, h)
            qp.setPen(QColor(255, 175, 175))
            qp.setBrush(QColor(255, 175, 175))
            qp.drawRect(full, 0, till - full, h)

        else:

            qp.setPen(QColor(255, 255, 255))
            qp.setBrush(QColor(255, 255, 184))
            qp.drawRect(0, 0, till, h)

        pen = QPen(QColor(20, 20, 20), 1,
                   Qt.PenStyle.SolidLine)

        qp.setPen(pen)
        qp.setBrush(Qt.BrushStyle.NoBrush)
        qp.drawRect(0, 0, w - 1, h - 1)

        j = 0

        for i in range(step, 10 * step, step):

            qp.drawLine(i, 0, i, 5)
            metrics = qp.fontMetrics()
            fw = metrics.horizontalAdvance(str(self.num[j]))

            x, y = int(i - fw/2), int(h / 2)
            qp.drawText(x, y, str(self.num[j]))
            j = j + 1


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        OVER_CAPACITY = 750

        sld = QSlider(Qt.Orientation.Horizontal, self)
        sld.setFocusPolicy(Qt.FocusPolicy.NoFocus)
        sld.setRange(1, OVER_CAPACITY)
        sld.setValue(75)
        sld.setGeometry(30, 40, 150, 30)

        self.c = Communicate()
        self.wid = BurningWidget()
        self.c.updateBW[int].connect(self.wid.setValue)

        sld.valueChanged[int].connect(self.changeValue)
        hbox = QHBoxLayout()
        hbox.addWidget(self.wid)
        vbox = QVBoxLayout()
        vbox.addStretch(1)
        vbox.addLayout(hbox)
        self.setLayout(vbox)

        self.setGeometry(300, 300, 390, 210)
        self.setWindowTitle('Burning widget')
        self.show()


    def changeValue(self, value):

        self.c.updateBW.emit(value)
        self.wid.repaint()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的例子中，我们有一个QSlider和一个自定义控件。滑块控制自定义控件。这个控件以图形方式显示了媒体介质的总容量和可用的空闲空间。我们自定义控件的最小值是1，最大值是OVER_CAPACITY。如果我们达到MAX_CAPACITY值，我们开始用红色绘制。这通常表示超刻。

烧录控件位于窗口的底部。这是通过一个QHBoxLayout和一个QVBoxLayout实现的。

```python
class BurningWidget(QWidget):

    def __init__(self):
        super().__init__()
```

烧录控件它基于QWidget控件。

```python
self.setMinimumSize(1, 30)
```

我们改变了控件的最小大小(高度)。默认值对我们来说有点小。

```python
font = QFont('Serif', 7, QFont.Weight.Light)
qp.setFont(font)
```

我们使用比默认字体更小的字体。这更适合我们的需要。

```python
size = self.size()
w = size.width()
h = size.height()

step = int(round(w / 10))


till = int(((w / OVER_CAPACITY) * self.value))
full = int(((w / OVER_CAPACITY) * MAX_CAPACITY))
```

控件采用了动态绘制技术。窗体越大，控件也随之变大；反之亦然。这也是我们需要计算自定义控件的载体控件(即窗体)尺寸的原因。till参数定义了需要绘制的总尺寸，它根据slider控件计算得出，是整体区域的比例值。full参数定义了红色区域的绘制起点。注意在绘制时为取得较大精度而使用的浮点数运算。

实际的绘制分三个步骤。黄色或红黄矩形的绘制，然后是刻度线的绘制，最后是刻度值的绘制。

```python
metrics = qp.fontMetrics()
fw = metrics.horizontalAdvance(str(self.num[j]))

x, y = int(i - fw/2), int(h / 2)
qp.drawText(x, y, str(self.num[j]))
```

我们使用字体度量来绘制文本。为了使文本以垂直线为中心，我们必须知道文本的宽度。

```python
def changeValue(self, value):

    self.c.updateBW.emit(value)
    self.wid.repaint()
```

当我们移动滑块时，会调用changeValue方法。在该方法内部，我们发送一个带有参数的自定义updateBW信号。该参数是滑块的当前值。该值稍后用于计算要绘制的烧录控件的容量。然后将重新绘制自定义控件。

![image-20210619210157102](F:\programming\Cplus\Qt\PyQt6-tutorial\Custom widgets\img\image-20210619210157102.png)

在PyQt6教程的这一部分中，我们创建了一个自定义控件。

[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Painting.md) [下一章]()