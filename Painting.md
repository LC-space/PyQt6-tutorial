[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Dialogs.md) [下一章]()

# PyQt6中的绘图

*最近更新于2021年5月15日*

PyQt6绘图系统能够渲染矢量图形、图像和基于轮廓字体的文本。当我们想要更改或增强现有的控件，或者从头创建自定义控件时，在应用程序中需要绘图。为了绘图，我们使用PyQt6工具箱提供的绘图API。

## QPainter

QPainter在控件和其他绘图设备上执行低级绘图。从简单的线条到复杂的形状，它都能画。

## paintEvent方法

绘图是在paintEvent方法中完成的。绘图代码放置在QPainter对象的开始和结束方法之间。它在控件和其他绘图设备上执行低级绘图。

## PyQt6绘制文本

我们首先在窗口的客户端区域绘制一些Unicode文本。

```python
# draw_text.py
# #!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we draw text in Russian Cylliric.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QWidget, QApplication
from PyQt6.QtGui import QPainter, QColor, QFont
from PyQt6.QtCore import Qt


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.text = "Лев Николаевич Толстой\nАнна Каренина"

        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Drawing text')
        self.show()


    def paintEvent(self, event):

        qp = QPainter()
        qp.begin(self)
        self.drawText(event, qp)
        qp.end()


    def drawText(self, event, qp):

        qp.setPen(QColor(168, 34, 3))
        qp.setFont(QFont('Decorative', 10))
        qp.drawText(event.rect(), Qt.AlignmentFlag.AlignCenter, self.text)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们用西利克语绘制一些文本。文本是垂直和水平对齐的。

```python
def paintEvent(self, event):
...
```

绘图是在绘制事件中完成的。

```python
qp = QPainter()
qp.begin(self)
self.drawText(event, qp)
qp.end()
```

QPainter类负责所有低级的绘制。所有的绘图方法都在开始方法和结束方法之间。实际的绘图被委托给drawText方法。

```python
qp.setPen(QColor(168, 34, 3))
qp.setFont(QFont('Decorative', 10))
```

在这里，我们定义了用于绘制文本的钢笔和字体。

```python
qp.drawText(event.rect(), Qt.AlignmentFlag.AlignCenter, self.text)
```

drawText方法在窗口上绘制文本。paint事件的rect方法返回需要更新的矩形。使用Qt.AlignmentFlag.Aligncenter，我们在两个维度上对齐文本。

![image-20210618225250881](F:\programming\Cplus\Qt\PyQt6-tutorial\Painting\img\image-20210618225250881.png)

## PyQt6画点

点是可以绘制的最简单的图形对象。它是窗口上的一个小点。

```python
# draw_points.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In the example, we draw randomly 1000 red points
on the window.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import QWidget, QApplication
from PyQt6.QtGui import QPainter
from PyQt6.QtCore import Qt
import sys, random


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setMinimumSize(50, 50)
        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Points')
        self.show()


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        self.drawPoints(qp)
        qp.end()


    def drawPoints(self, qp):

        qp.setPen(Qt.GlobalColor.red)
        size = self.size()

        for i in range(1000):

            x = random.randint(1, size.width() - 1)
            y = random.randint(1, size.height() - 1)
            qp.drawPoint(x, y)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们在窗口的客户端区域随机绘制1000个红色点。

```python
qp.setPen(Qt.GlobalColor.red)
```

我们把钢笔调成红色。我们使用一个预定义的Qt.GlobalColor.red颜色常量。

```python
size = self.size()
```

每次我们调整窗口的大小，就会生成一个绘制事件。我们通过size方法获得窗口的当前大小。我们使用窗口的大小来分配所有的点在窗口的客户区域。

```python
qp.drawPoint(x, y)
```

我们用drawPoint方法绘制这个点。

![image-20210618225620602](F:\programming\Cplus\Qt\PyQt6-tutorial\Painting\img\image-20210618225620602.png)

## PyQt6颜色

颜色是表示红、绿、蓝（RGB）强度值组合的对象。有效的RGB值范围是0到255。我们可以用不同的方法定义一种颜色。最常见的是RGB十进制值或十六进制值。我们也可以使用RGBA值来代表红色、绿色、蓝色和Alpha。这里我们添加了一些关于透明度的额外信息。Alpha值255定义完全不透明度，0表示完全透明，例如颜色是不可见的。

```python
# colours.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example draws three rectangles in three
different colours.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import QWidget, QApplication
from PyQt6.QtGui import QPainter, QColor
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 350, 100)
        self.setWindowTitle('Colours')
        self.show()


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        self.drawRectangles(qp)
        qp.end()


    def drawRectangles(self, qp):

        col = QColor(0, 0, 0)
        col.setNamedColor('#d4d4d4')
        qp.setPen(col)

        qp.setBrush(QColor(200, 0, 0))
        qp.drawRect(10, 15, 90, 60)

        qp.setBrush(QColor(255, 80, 0, 160))
        qp.drawRect(130, 15, 90, 60)

        qp.setBrush(QColor(25, 0, 90, 200))
        qp.drawRect(250, 15, 90, 60)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的例子中，我们绘制了三个彩色矩形。

```python
color = QColor(0, 0, 0)
color.setNamedColor('#d4d4d4')
```

这里我们使用十六进制表示法定义颜色。

```python
qp.setBrush(QColor(200, 0, 0))
qp.drawRect(10, 15, 90, 60)
```

这里我们定义一个画笔并绘制一个矩形。笔刷是一种基本的图形对象，用于绘制形状的背景。drawRect方法接受四个参数。前两个是轴上的x和y值。第三和第四个参数是矩形的宽度和高度。该方法使用当前的钢笔和画笔绘制矩形。

![image-20210618225744951](F:\programming\Cplus\Qt\PyQt6-tutorial\Painting\img\image-20210618225744951.png)

## PyQt6 QPen

QPen是一个基本的图形对象。它用于绘制直线、曲线和矩形、椭圆、多边形或其他形状的轮廓。

```python
# pens.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example we draw 6 lines using
different pen styles.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import QWidget, QApplication
from PyQt6.QtGui import QPainter, QPen
from PyQt6.QtCore import Qt
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 280, 270)
        self.setWindowTitle('Pen styles')
        self.show()


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        self.drawLines(qp)
        qp.end()


    def drawLines(self, qp):

        pen = QPen(Qt.GlobalColor.black, 2, Qt.PenStyle.SolidLine)

        qp.setPen(pen)
        qp.drawLine(20, 40, 250, 40)

        pen.setStyle(Qt.PenStyle.DashLine)
        qp.setPen(pen)
        qp.drawLine(20, 80, 250, 80)

        pen.setStyle(Qt.PenStyle.DashDotLine)
        qp.setPen(pen)
        qp.drawLine(20, 120, 250, 120)

        pen.setStyle(Qt.PenStyle.DotLine)
        qp.setPen(pen)
        qp.drawLine(20, 160, 250, 160)

        pen.setStyle(Qt.PenStyle.DashDotDotLine)
        qp.setPen(pen)
        qp.drawLine(20, 200, 250, 200)

        pen.setStyle(Qt.PenStyle.CustomDashLine)
        pen.setDashPattern([1, 4, 5, 4])
        qp.setPen(pen)
        qp.drawLine(20, 240, 250, 240)


def main():
    
    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的例子中，我们画了六条线。这些线条有六种不同的笔法。有五种预定义的钢笔风格。我们也可以创建自定义的钢笔样式。最后一条线是使用自定义钢笔风格绘制的。

```python
pen = QPen(Qt.GlobalColor.black, 2, Qt.PenStyle.SolidLine)
```

我们创建一个QPen对象。颜色是黑色的。宽度设置为2像素，以便我们可以看到钢笔风格之间的差异。solidline是预定义的钢笔样式之一。

```python
pen.setStyle(Qt.PenStyle.CustomDashLine)
pen.setDashPattern([1, 4, 5, 4])
qp.setPen(pen)
```

这里我们定义了一个自定义的钢笔样式。我们设置一个Qt.PenStyle.CustomDashLine钢笔样式，并调用setDashPattern方法。数字列表定义了一种风格。必须有偶数个数字。奇数定义破折号，偶数定义空格。数字越大，空格或破折号就越大。我们的图案是1px短划，4px间距，5px短划，4px间距等等。

![image-20210618230227377](F:\programming\Cplus\Qt\PyQt6-tutorial\Painting\img\image-20210618230227377.png)

## PyQt6 QBrush

QBrush是一个基本的图形对象。它用于绘制图形形状的背景，如矩形、椭圆或多边形。一个笔刷可以有三种不同的类型：一个预定义的笔刷，一个渐变，或者一个纹理模式。

```python
# brushes.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example draws nine rectangles in different
brush styles.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import QWidget, QApplication
from PyQt6.QtGui import QPainter, QBrush
from PyQt6.QtCore import Qt
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 355, 280)
        self.setWindowTitle('Brushes')
        self.show()


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        self.drawBrushes(qp)
        qp.end()


    def drawBrushes(self, qp):

        brush = QBrush(Qt.BrushStyle.SolidPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 15, 90, 60)

        brush.setStyle(Qt.BrushStyle.Dense1Pattern)
        qp.setBrush(brush)
        qp.drawRect(130, 15, 90, 60)

        brush.setStyle(Qt.BrushStyle.Dense2Pattern)
        qp.setBrush(brush)
        qp.drawRect(250, 15, 90, 60)

        brush.setStyle(Qt.BrushStyle.DiagCrossPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 105, 90, 60)

        brush.setStyle(Qt.BrushStyle.Dense5Pattern)
        qp.setBrush(brush)
        qp.drawRect(130, 105, 90, 60)

        brush.setStyle(Qt.BrushStyle.Dense6Pattern)
        qp.setBrush(brush)
        qp.drawRect(250, 105, 90, 60)

        brush.setStyle(Qt.BrushStyle.HorPattern)
        qp.setBrush(brush)
        qp.drawRect(10, 195, 90, 60)

        brush.setStyle(Qt.BrushStyle.VerPattern)
        qp.setBrush(brush)
        qp.drawRect(130, 195, 90, 60)

        brush.setStyle(Qt.BrushStyle.BDiagPattern)
        qp.setBrush(brush)
        qp.drawRect(250, 195, 90, 60)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的例子中，我们画了9个不同的矩形。

```python
brush = QBrush(Qt.BrushStyle.SolidPattern)
qp.setBrush(brush)
qp.drawRect(10, 15, 90, 60)
```

我们定义一个画笔对象。我们将其设置为画家对象，并通过调用drawRect方法绘制矩形。

![image-20210618230420456](F:\programming\Cplus\Qt\PyQt6-tutorial\Painting\img\image-20210618230420456.png)

## 贝塞尔曲线

贝塞尔曲线是一条三次直线。可以使用QPainterPath创建PyQt6中的贝塞尔曲线。画家路径是由许多图形构建块（如矩形、椭圆、直线和曲线）组成的对象。

```python
# bezier_curve.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program draws a Bézier curve with
QPainterPath.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtGui import QPainter, QPainterPath
from PyQt6.QtWidgets import QWidget, QApplication


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.setGeometry(300, 300, 380, 250)
        self.setWindowTitle('Bézier curve')
        self.show()


    def paintEvent(self, e):

        qp = QPainter()
        qp.begin(self)
        qp.setRenderHint(QPainter.RenderHint.Antialiasing)
        self.drawBezierCurve(qp)
        qp.end()


    def drawBezierCurve(self, qp):
    
        path = QPainterPath()
        path.moveTo(30, 30)
        path.cubicTo(30, 30, 200, 350, 350, 30)

        qp.drawPath(path)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

这个例子绘制了一个贝塞尔曲线。

```python
path = QPainterPath()
path.moveTo(30, 30)
path.cubicTo(30, 30, 200, 350, 350, 30)
```

我们使用QPainterPath路径创建了一个贝塞尔曲线。这条曲线是用cubicTo方法创建的，它取三个点：起点、控制点和终点。

```python
qp.drawPath(path)
```

最终的路径是用drawPath方法绘制的。

![image-20210618230729351](F:\programming\Cplus\Qt\PyQt6-tutorial\Painting\img\image-20210618230729351.png)

在PyQt6教程的这一部分中，我们做了一些基本的绘画。

[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Dialogs.md) [下一章]()