[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Menus%20and%20toolbars.md) [下一章]()

# PyQt6中的布局管理

*最近更新于2021年4月27日*

布局管理是我们在应用程序窗口中放置控件的方式。我们可以使用绝对定位或布局类来放置控件。使用布局管理器管理布局是组织控件的首选方式。

## 绝对定位

程序员以像素为单位指定每个控件的位置和大小。当你使用绝对定位时，我们必须了解以下限制：

* 如果我们调整窗口的大小，控件的大小和位置不会改变
* 应用程序在不同的平台上可能看起来不同
* 改变应用程序中的字体可能会破坏布局
* 如果我们决定改变我们的布局，我们必须完全重做我们的布局，这是乏味和耗时的

下面的示例以绝对坐标来定位控件。

```python
# absolute.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This example shows three labels on a window
using absolute positioning.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import QWidget, QLabel, QApplication


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        lbl1 = QLabel('ZetCode', self)
        lbl1.move(15, 10)

        lbl2 = QLabel('tutorials', self)
        lbl2.move(35, 40)

        lbl3 = QLabel('for programmers', self)
        lbl3.move(55, 70)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Absolute')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

我们使用move方法来定位控件。在我们的例子中，这些是标签。我们通过提供x和y坐标来定位它们。坐标系的起点在左上角。x值从左到右递增。y值从上到下递增。

```python
lbl1 = QLabel('ZetCode', self)
lbl1.move(15, 10)
```

标签控件位于x=15和y=10。

![image-20210614160258237](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Layout%20management/img/image-20210614160258237.png)

## PyQt6 QHBoxLayout

QHBoxLayout和QVBoxLayout是基本的布局类，可以水平和垂直排列控件。

假设我们想在右下角放置两个按钮。要创建这样的布局，我们使用一个水平框和一个垂直框。为了创造必要的空间，我们添加了一个伸缩控件。

```python
# box_layout.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we position two push
buttons in the bottom-right corner
of the window.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import (QWidget, QPushButton,
        QHBoxLayout, QVBoxLayout, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        okButton = QPushButton("OK")
        cancelButton = QPushButton("Cancel")

        hbox = QHBoxLayout()
        hbox.addStretch(1)
        hbox.addWidget(okButton)
        hbox.addWidget(cancelButton)

        vbox = QVBoxLayout()
        vbox.addStretch(1)
        vbox.addLayout(hbox)

        self.setLayout(vbox)

        self.setGeometry(300, 300, 350, 250)
        self.setWindowTitle('Buttons')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

该示例在窗口的右下角放置了两个按钮。当我们调整应用程序窗口的大小时，它们仍然在那里。我们同时使用HBoxLayout和QVBoxLayout。

```python
okButton = QPushButton("OK")
cancelButton = QPushButton("Cancel")
```

这里我们创建了两个按钮。

```python
hbox = QHBoxLayout()
hbox.addStretch(1)
hbox.addWidget(okButton)
hbox.addWidget(cancelButton)
```

我们创建一个水平框布局，添加一个伸缩控件和两个按钮。拉伸在两个按钮之前增加了一个可伸缩的空间。这会把他们推到窗口的右边。

```python
vbox = QVBoxLayout()
vbox.addStretch(1)
vbox.addLayout(hbox)
```

水平布局被置于垂直布局中。垂直框中的伸缩控件将把带有按钮的水平框推到窗口的底部。

```python
self.setLayout(vbox)
```

最后，我们设置了窗口的主布局。

![image-20210614161426013](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Layout%20management/img/image-20210614161426013.png)

## PyQt6 QGridLayout

QGridLayout是最通用的布局类。它将空间分成行和列。

```python
# calculator.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we create a skeleton
of a calculator using QGridLayout.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import (QWidget, QGridLayout,
        QPushButton, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        grid = QGridLayout()
        self.setLayout(grid)

        names = ['Cls', 'Bck', '', 'Close',
                 '7', '8', '9', '/',
                 '4', '5', '6', '*',
                 '1', '2', '3', '-',
                 '0', '.', '=', '+']

        positions = [(i, j) for i in range(5) for j in range(4)]

        for position, name in zip(positions, names):

            if name == '':
                continue

            button = QPushButton(name)
            grid.addWidget(button, *position)

        self.move(300, 150)
        self.setWindowTitle('Calculator')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们创建了一个按钮网格。

```python
grid = QGridLayout()
self.setLayout(grid)
```

创建QGridLayout的实例并将其设置为应用程序窗口的布局。

```python
names = ['Cls', 'Bck', '', 'Close',
         '7', '8', '9', '/',
         '4', '5', '6', '*',
         '1', '2', '3', '-',
         '0', '.', '=', '+']
```

这些是后来用于按钮的标签。

```python
positions = [(i,j) for i in range(5) for j in range(4)]
```

我们在网格中创建一个位置列表。

```python
for position, name in zip(positions, names):
    
    if name == '':
        continue
        
    button = QPushButton(name)
    grid.addWidget(button, *position)
```

使用addWidget方法创建按钮并将其添加到布局中。

![image-20210614162316241](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Layout%20management/img/image-20210614162316241.png)

## 评论的例子

控件可以跨越网格中的多个列或行。在下一个示例中，我们将对此进行说明。

```python
# review.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we create a bit
more complicated window layout using
the QGridLayout manager.

Author: Jan Bodnar
Website: zetcode.com
"""

import sys
from PyQt6.QtWidgets import (QWidget, QLabel, QLineEdit,
        QTextEdit, QGridLayout, QApplication)


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        title = QLabel('Title')
        author = QLabel('Author')
        review = QLabel('Review')

        titleEdit = QLineEdit()
        authorEdit = QLineEdit()
        reviewEdit = QTextEdit()

        grid = QGridLayout()
        grid.setSpacing(10)

        grid.addWidget(title, 1, 0)
        grid.addWidget(titleEdit, 1, 1)

        grid.addWidget(author, 2, 0)
        grid.addWidget(authorEdit, 2, 1)

        grid.addWidget(review, 3, 0)
        grid.addWidget(reviewEdit, 3, 1, 5, 1)

        self.setLayout(grid)

        self.setGeometry(300, 300, 350, 300)
        self.setWindowTitle('Review')
        self.show()


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

我们创建一个窗口，其中有三个标签、两个行编辑和一个文本编辑控件。布局是通过QGridLayout完成的。

```python
grid = QGridLayout()
grid.setSpacing(10)
```

我们创建一个网格布局并设置控件之间的间距。

```python
grid.addWidget(reviewEdit, 3, 1, 5, 1)
```

如果将控件添加到网格中，则可以提供控件的行跨度和列跨度。在我们的例子中，我们让reviewEdit控件跨越5行。

![image-20210614162945125](https://raw.githubusercontent.com/LC-space/PyQt6-tutorial/main/Layout%20management/img/image-20210614162945125.png)

PyQt6教程的这一部分专注于布局管理。