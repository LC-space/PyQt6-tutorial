[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Widgets.md) [下一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Drag%20%26%20drop.md)

# PyQt6控件2

*最近更新于2021年5月5日*

在本章中，我们继续介绍PyQt6控件。我们涵盖了QPixmap, QLineEdit, QSplitter和QComboBox。

## PyQt6 QPixmap

QPixmap是用于处理图像的控件之一。它为在屏幕上显示图像进行了优化。在我们的代码示例中，我们将使用QPixmap在窗口上显示图像。

```python
# pixmap.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we display an image
on the window.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QHBoxLayout,
        QLabel, QApplication)
from PyQt6.QtGui import QPixmap
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        hbox = QHBoxLayout(self)
        pixmap = QPixmap('img/sid.jpg')

        lbl = QLabel(self)
        lbl.setPixmap(pixmap)

        hbox.addWidget(lbl)
        self.setLayout(hbox)

        self.move(300, 200)
        self.setWindowTitle('Sid')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们在窗口上显示一个图像。

```python
pixmap = QPixmap('img/sid.jpg')
```

我们创建一个QPixmap对象。它以文件名作为参数。

```python
lbl = QLabel(self)
lbl.setPixmap(pixmap)
```

我们将pixmap放到QLabel控件中。

## PyQt6 QLineEdit

QLineEdit是一个控件，它允许输入和编辑单行纯文本。这个控件有撤消和重做，剪切和粘贴，以及拖放功能。

```python
# line_edit.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows text which
is entered in a QLineEdit
in a QLabel widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import (QWidget, QLabel,
        QLineEdit, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.lbl = QLabel(self)
        qle = QLineEdit(self)

        qle.move(60, 100)
        self.lbl.move(60, 40)

        qle.textChanged[str].connect(self.onChanged)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('QLineEdit')
        self.show()


    def onChanged(self, text):

        self.lbl.setText(text)
        self.lbl.adjustSize()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

这个例子显示了一个行编辑控件和一个标签。我们在行编辑中键入的文本立即显示在标签控件中。

```python
qle = QLineEdit(self)
```

创建了QLineEdit控件。

```python
qle.textChanged[str].connect(self.onChanged)
```

如果行编辑控件中的文本发生了变化，我们就调用onChanged方法。

```python
def onChanged(self, text):
    
    self.lbl.setText(text)
    self.lbl.adjustSize() 
```

在onChanged方法内部，我们将类型化文本设置为标签控件。我们调用adjustSize方法来调整标签的大小以适应文本的长度。

![image-20210618102845014](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets%20II/img/image-20210618102845014.png)

## PyQt6 QSplitter

QSplitter允许用户通过拖动控件之间的边界来控制子控件的大小。在我们的示例中，我们展示了用两个分割器组织的三个QFrame控件。

```python
# splitter.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows
how to use QSplitter widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtCore import Qt
from PyQt6.QtWidgets import (QWidget, QHBoxLayout, QFrame,
        QSplitter, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        hbox = QHBoxLayout(self)

        topleft = QFrame(self)
        topleft.setFrameShape(QFrame.Shape.StyledPanel)

        topright = QFrame(self)
        topright.setFrameShape(QFrame.Shape.StyledPanel)

        bottom = QFrame(self)
        bottom.setFrameShape(QFrame.Shape.StyledPanel)

        splitter1 = QSplitter(Qt.Orientation.Horizontal)
        splitter1.addWidget(topleft)
        splitter1.addWidget(topright)

        splitter2 = QSplitter(Qt.Orientation.Vertical)
        splitter2.addWidget(splitter1)
        splitter2.addWidget(bottom)

        hbox.addWidget(splitter2)
        self.setLayout(hbox)

        self.setGeometry(300, 300, 450, 400)
        self.setWindowTitle('QSplitter')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们有三个框架控件和两个分割器。请注意，在某些主题下，分割器可能不是可见的。

```python
topleft = QFrame(self)
topleft.setFrameShape(QFrame.Shape.StyledPanel)
```

为了查看QFrame控件之间的边界，我们使用了一个样式化的框架。

```python
plitter1 = QSplitter(Qt.Orientations.Horizontal)
splitter1.addWidget(topleft)
splitter1.addWidget(topright)
```

我们创建一个QSplitter控件并向其添加两个框架。

```python
splitter2 = QSplitter(Qt.Orientations.Vertical)
splitter2.addWidget(splitter1)
```

我们还可以向另一个分割器控件添加分割器。

![image-20210618105544854](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets%20II/img/image-20210618105544854.png)

## PyQt6 QComboBox

QComboBox是一个控件，允许用户从选项列表中进行选择。

```python
# combobox.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows how to use
a QComboBox widget.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys

from PyQt6.QtWidgets import (QWidget, QLabel,
        QComboBox, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.lbl = QLabel('Ubuntu', self)

        combo = QComboBox(self)

        combo.addItem('Ubuntu')
        combo.addItem('Mandriva')
        combo.addItem('Fedora')
        combo.addItem('Arch')
        combo.addItem('Gentoo')

        combo.move(50, 50)
        self.lbl.move(50, 150)

        combo.textActivated[str].connect(self.onActivated)

        self.setGeometry(300, 300, 450, 400)
        self.setWindowTitle('QComboBox')
        self.show()


    def onActivated(self, text):

        self.lbl.setText(text)
        self.lbl.adjustSize()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

这个例子显示了一个QComboBox和一个QLabel。组合框有一个包含五个选项的列表。这些是Linux发行版的名称。标签控件显示组合框中选择的选项。

```python
combo = QComboBox(self)

combo.addItem('Ubuntu')
combo.addItem('Mandriva')
combo.addItem('Fedora')
combo.addItem('Arch')
combo.addItem('Gentoo')
```

我们创建了一个带有五个选项的QComboBox控件。

```python
combo.textActivated[str].connect(self.onActivated)
```

在项目选择之后，我们调用onActivated方法。

```python
def onActivated(self, text):
  
    self.lbl.setText(text)
    self.lbl.adjustSize() 
```

在该方法内部，我们将所选项目的文本设置为标签控件。我们调整标签的大小。

![image-20210618110033614](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Widgets%20II/img/image-20210618110033614.png)

在PyQt6教程的这一部分，我们已经讨论了QPixmap, QLineEdit, QSplitter和QComboBox。

[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Widgets.md) [下一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Drag%20%26%20drop.md)
