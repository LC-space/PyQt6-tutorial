[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Date%20and%20time.md) [下一章]()

# PyQt6中的第一个程序

*最近更新于2021年4月23日*

在PyQt6教程的这一部分中，我们将学习一些基本的功能。这些示例显示了一个提示信息和一个图标，关闭一个窗口，显示一个消息框，并在桌面上显示一个窗口。

## PyQt6简单的例子

这是一个显示小窗口的简单示例。然而，我们可以利用这个窗口做很多事情。我们可以调整它的大小，最大化或最小化。这需要大量的编码。有人已经编写了这个功能。因为它在大多数应用程序中都是重复的，所以不需要重新编写代码。PyQt6是一个高级工具包。如果我们在较低级别的工具包中编写代码，下面的代码示例很容易有数百行代码。

```python
# simple.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we create a simple
window in PyQt6.

Author: Jan Bodnar
Website: zetcode.com
"""


import sys
from PyQt6.QtWidgets import QApplication, QWidget


def main():

    app = QApplication(sys.argv)

    w = QWidget()
    w.resize(250, 200)
    w.move(300, 300)

    w.setWindowTitle('Simple')
    w.show()

    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

上面的代码示例在屏幕上显示了一个小窗口。

```python
import sys
from PyQt6.QtWidgets import QApplication, QWidget
```

这里我们提供了必要的导入。基本控件位于PyQt6中。QtWidgets模块。

```python
app = QApplication(sys.argv)
```

每个PyQt6应用程序都必须创建一个应用程序对象。sys.Argv参数是来自命令行的参数列表。Python脚本可以从shell运行。这是我们控制脚本启动的一种方式。

```python
w = QWidget()
```

QWidget控件是PyQt6中所有用户接口对象的基类。我们为QWidget提供了默认构造函数。默认构造函数没有父类。没有父组件的控件称为窗口。

```python
w.resize(250, 150)
```

resize方法调整控件的大小。它是250px宽，150px高。

```python
w.move(300, 300)
```

move方法将小部件移动到屏幕上坐标为x=300， y=300的位置。

```python
w.setWindowTitle('Simple')
```

我们使用setWindowTitle设置窗口的标题。标题显示在标题栏中。

```python
w.show()
```

show方法将控件显示在屏幕上。控件首先在内存中创建，然后显示在屏幕上。

```python
sys.exit(app.exec())
```

最后，我们进入应用程序的主循环。事件处理从这里开始。主循环从窗口系统接收事件，并将它们分派给应用程序控件。如果我们调用exit方法，或者主控件被关闭，则主循环结束。sys.exit方法确保完全的退出。将告知环境应用程序是如何结束的。

![image-20210613145725126](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/First%20programs/img/image-20210613145725126.png)

## PyQt6提示信息

我们可以为任何控件提供气泡帮助。

```python
# tooltip.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows a tooltip on
a window and a button.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import (QWidget, QToolTip,
    QPushButton, QApplication)
from PyQt6.QtGui import QFont


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        QToolTip.setFont(QFont('SansSerif', 10))

        self.setToolTip('This is a <b>QWidget</b> widget')

        btn = QPushButton('Button', self)
        btn.setToolTip('This is a <b>QPushButton</b> widget')
        btn.resize(btn.sizeHint())
        btn.move(50, 50)

        self.setGeometry(300, 300, 300, 200)
        self.setWindowTitle('Tooltips')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在这个例子中，我们展示了两个PyQt6控件的提示信息。

```python
QToolTip.setFont(QFont('SansSerif', 10))
```

此静态方法设置用于呈现提示信息的字体。我们使用10pt SansSerif字体。

```python
self.setToolTip('This is a <b>QWidget</b> widget')
```

要创建提示信息，我们调用setTooltip方法。我们可以使用富文本格式。

```python
btn = QPushButton('Button', self)
btn.setToolTip('This is a <b>QPushButton</b> widget')
```

我们创建一个按钮部件并为它设置一个工具提示。

```python
btn.resize(btn.sizeHint())
btn.move(50, 50)
```

调整窗口上按钮的大小和移动。sizeHint方法给出了按钮的推荐大小。

![image-20210613151219139](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/First%20programs/img/image-20210613151219139.png)

## PyQt6退出按钮

关闭窗口最明显的方法是单击标题栏上的x标记。在下一个示例中，我们将展示如何以编程方式关闭窗口。我们将简要地讨论信号和槽。

下面是我们在示例中使用的QPushButton小部件的构造函数。

```python
QPushButton(string text, QWidget parent = None)
```

text参数是将显示在按钮上的文本。parent是放置按钮的控件。在我们的例子中，它将是一个QWidget。应用程序的控件形成了层次结构。在这个层次结构中，大多数控件都有它们的父控件。没有父控件的控件是顶层窗口。

```python
# quit_button.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program creates a quit
button. When we press the button,
the application terminates.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QWidget, QPushButton, QApplication

class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        qbtn = QPushButton('Quit', self)
        qbtn.clicked.connect(QApplication.instance().quit)
        qbtn.resize(qbtn.sizeHint())
        qbtn.move(50, 50)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Quit button')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在本例中，我们创建了一个退出按钮。单击按钮后，应用程序终止。

```python
qbtn = QPushButton('Quit', self)
```

我们创造了一个按钮。按钮是QPushButton类的一个实例。构造函数的第一个参数是按钮的标签。第二个参数是父控件。父控件是Example控件，它是一个继承的QWidget。

```python
qbtn.clicked.connect(QApplication.instance().quit)
```

PyQt6中的事件处理系统是用信号&槽机制构建的。如果我们点击按钮，就会发出被点击的信号。槽可以是Qt槽或任何Python可调用对象。

QCoreApplication，它是用QApplication.instance检索的。包含主事件循环——它处理和分派所有事件。单击的信号连接到终止应用程序的quit方法。通信是在两个对象之间完成的：发送方和接收方。发送方是按钮，接收方是应用程序对象。

![image-20210613152347253](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/First%20programs/img/image-20210613152347253.png)

## PyQt6消息框

默认情况下，如果我们单击标题栏上的x按钮，QWidget是会关闭的。有时我们想要修改这个默认行为。例如，如果我们在编辑器中打开了一个文件，我们对它做了一些更改。我们将显示一个消息框来确认操作。

```python
# messagebox.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program shows a confirmation
message box when we click on the close
button of the application window.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QWidget, QMessageBox, QApplication


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()

    def initUI(self):

        self.setGeometry(300, 300, 350, 200)
        self.setWindowTitle('Message box')
        self.show()

    def closeEvent(self, event):

        reply = QMessageBox.question(self, 'Message',
                    "Are you sure to quit?", QMessageBox.StandardButton.Yes |
                    QMessageBox.StandardButton.No, QMessageBox.StandardButton.No)

        if reply == QMessageBox.StandardButton.Yes:

            event.accept()
        else:

            event.ignore()


def main():
    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

如果我们关闭一个QWidget，就会生成QCloseEvent。要修改控件行为，我们需要重新实现closeEvent事件处理程序。

```python
reply = QMessageBox.question(self, 'Message',
                    "Are you sure to quit?", QMessageBox.StandardButton.Yes |
                    QMessageBox.StandardButton.No, QMessageBox.StandardButton.No)
```

我们将显示一个带有两个按钮的消息框：Yes和No。第一个字符串出现在标题栏上。第二个字符串是对话框显示的消息文本。第三个参数指定对话框中出现的按钮组合。最后一个参数是默认按钮。它是最初具有键盘焦点的按钮。返回值存储在reply变量中。

```python
if reply == QMessageBox.StandardButton.Yes:
	event.accept()
else:
	event.ignore()
```

这里我们测试返回值。如果单击Yes按钮，就会接受导致控件关闭和应用程序终止的事件。否则我们将忽略关闭事件。

![image-20210613160315565](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/First%20programs/img/image-20210613160315565.png)

## PyQt6中心窗口

下面的脚本展示了如何在桌面屏幕上居中一个窗口。

```python
# center.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This program centers a window
on the screen.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QWidget, QApplication


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()

    def initUI(self):

        self.resize(350, 250)
        self.center()

        self.setWindowTitle('Center')
        self.show()

    def center(self):

        qr = self.frameGeometry()
        cp = self.screen().availableGeometry().center()

        qr.moveCenter(cp)
        self.move(qr.topLeft())


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

QScreen类用于查询屏幕属性。

```python
self.center()
```

将窗口居中的代码放置在自定义中心方法中。

```python
qr = self.frameGeometry()
```

我们得到一个指定主窗口几何形状的矩形。这包括任何窗口框架。

```python
cp = self.screen().availableGeometry().center()
```

我们计算出显示器的屏幕分辨率。通过这个分辨率，我们得到了中心点。

```python
qr.moveCenter(cp)
```

我们的矩形已经有了它的宽和高。现在我们将矩形的中心设置为屏幕的中心。矩形的大小不变。

```python
self.move(qr.topLeft())
```

我们将应用程序窗口的左上角移动到qr矩形的左上角，从而使窗口在屏幕上居中。

在PyQt6教程的这一部分中，我们已经在PyQt6中创建了简单的代码示例。