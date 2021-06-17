[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Events%20and%20signals.md) [下一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Widgets.md)

# PyQt6中的对话框

*最近更新于2021年4月30日*

对话被定义为两个或两个以上的人之间的对话。在计算机应用程序中，对话框是用来与应用程序“对话”的窗口。对话框用于从用户获取数据或更改应用程序设置。

## PyQt6 QInputDialog

QInputDialog提供了一个简单方便的对话框，从用户那里获取单个值。输入值可以是字符串、数字或列表中的项。

```python
# input_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we receive data from
a QInputDialog dialog.

Aauthor: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QPushButton, QLineEdit,
        QInputDialog, QApplication)
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.btn = QPushButton('Dialog', self)
        self.btn.move(20, 20)
        self.btn.clicked.connect(self.showDialog)

        self.le = QLineEdit(self)
        self.le.move(130, 22)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Input dialog')
        self.show()


    def showDialog(self):

        text, ok = QInputDialog.getText(self, 'Input Dialog',
                                        'Enter your name:')

        if ok:
            self.le.setText(str(text))


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

该示例有一个按钮和一个行编辑控件。该按钮显示用于获取文本值的输入对话框。输入的文本将显示在line edit控件中。

```python
text, ok = QInputDialog.getText(self, 'Input Dialog',
    'Enter your name:')
```

这一行显示输入对话框。第一个字符串是对话框标题，第二个是对话框中的消息。对话框返回输入的文本和一个布尔值。如果我们单击Ok按钮，布尔值为true。

```python
if ok:
    self.le.setText(str(text))
```

我们从对话框接收到的文本被设置为使用setText()的行编辑控件。

![image-20210617175356639](F:\programming\Cplus\Qt\PyQt6-tutorial\Dialogs\img\image-20210617175356639.png)

## PyQt6 QColorDialog

QColorDialog提供了一个用于选择颜色值的对话框控件。

```python
# color_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we select a color value
from the QColorDialog and change the background
color of a QFrame widget.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QPushButton, QFrame,
        QColorDialog, QApplication)
from PyQt6.QtGui import QColor
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        col = QColor(0, 0, 0)

        self.btn = QPushButton('Dialog', self)
        self.btn.move(20, 20)

        self.btn.clicked.connect(self.showDialog)

        self.frm = QFrame(self)
        self.frm.setStyleSheet("QWidget { background-color: %s }"
                               % col.name())
        self.frm.setGeometry(130, 22, 200, 200)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Color dialog')
        self.show()


    def showDialog(self):

        col = QColorDialog.getColor()

        if col.isValid():

            self.frm.setStyleSheet("QWidget { background-color: %s }" 
                                   % col.name())


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

用程序示例显示了一个按钮和一个QFrame。控件的背景设置为黑色。使用QColorDialog，我们可以改变它的背景。

```python
col = QColor(0, 0, 0)
```

这是QFrame背景的初始颜色。

```python
col = QColorDialog.getColor()
```

这一行弹出QColorDialog。

```python
if col.isValid():

    self.frm.setStyleSheet("QWidget { background-color: %s }" 
                           % col.name())
```

我们检查颜色是否有效。如果我们点击Cancel按钮，将不会返回有效的颜色。如果颜色有效，我们将使用样式表更改背景颜色。

## PyQt6 QFontDialog

QFontDialog是一个用于选择字体的对话框控件。

```python
# font_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we select a font name
and change the font of a label.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QWidget, QVBoxLayout, QPushButton,
        QSizePolicy, QLabel, QFontDialog, QApplication)
import sys


class Example(QWidget):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        vbox = QVBoxLayout()

        btn = QPushButton('Dialog', self)
        btn.setSizePolicy(QSizePolicy.Policy.Fixed, QSizePolicy.Policy.Fixed)
        btn.move(20, 20)

        vbox.addWidget(btn)

        btn.clicked.connect(self.showDialog)

        self.lbl = QLabel('Knowledge only matters', self)
        self.lbl.move(130, 20)

        vbox.addWidget(self.lbl)
        self.setLayout(vbox)

        self.setGeometry(300, 300, 450, 350)
        self.setWindowTitle('Font dialog')
        self.show()


    def showDialog(self):

        font, ok = QFontDialog.getFont()

        if ok:
            self.lbl.setFont(font)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

在我们的示例中，我们有一个按钮和一个标签。使用QFontDialog，我们改变标签的字体。

```python
font, ok = QFontDialog.getFont()
```

这里我们弹出字体对话框。getFont方法返回字体名称和ok参数。如果用户单击Ok，它等于True;否则为False。

```python
if ok:
    self.label.setFont(font)
```

如果我们点击Ok，标签的字体会被setFont改变。

## PyQt6 QFileDialog

QFileDialog是一个允许用户选择文件或目录的对话框。可以选择打开和保存文件。

```python
# file_dialog.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

In this example, we select a file with a
QFileDialog and display its contents
in a QTextEdit.

Author: Jan Bodnar
Website: zetcode.com
"""

from PyQt6.QtWidgets import (QMainWindow, QTextEdit,
        QFileDialog, QApplication)
from PyQt6.QtGui import QIcon, QAction
from pathlib import Path
import sys


class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):

        self.textEdit = QTextEdit()
        self.setCentralWidget(self.textEdit)
        self.statusBar()

        openFile = QAction(QIcon('img/open.png'), 'Open', self)
        openFile.setShortcut('Ctrl+O')
        openFile.setStatusTip('Open new File')
        openFile.triggered.connect(self.showDialog)

        menubar = self.menuBar()
        fileMenu = menubar.addMenu('&File')
        fileMenu.addAction(openFile)

        self.setGeometry(300, 300, 550, 450)
        self.setWindowTitle('File dialog')
        self.show()


    def showDialog(self):

        home_dir = str(Path.home())
        fname = QFileDialog.getOpenFileName(self, 'Open file', home_dir)

        if fname[0]:

            f = open(fname[0], 'r')

            with f:

                data = f.read()
                self.textEdit.setText(data)


def main():

    app = QApplication(sys.argv)
    ex = Example()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

该示例显示了一个菜单栏、集中设置的文本编辑控件和一个状态栏。菜单项显示用于选择文件的QFileDialog。文件的内容被加载到文本编辑控件中。

```python
class Example(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()
```

这个示例基于QMainWindow控件，因为我们集中设置了一个文本编辑控件。

```python
home_dir = str(Path.home())
fname = QFileDialog.getOpenFileName(self, 'Open file', home_dir)
```

我们弹出QFileDialog。getOpenFileName方法中的第一个字符串是标题。第二个字符串指定对话框的工作目录。我们使用path模块来确定用户的主目录。缺省情况下，文件过滤器设置为“所有文件(*)”。

```python
if fname[0]:

    f = open(fname[0], 'r')

    with f:

        data = f.read()
        self.textEdit.setText(data)
```

读取选定的文件名，并将文件的内容设置为文本编辑控件。

在PyQt6教程的这一部分中，我们使用了对话框。

[目录](https://github.com/LC-space/PyQt6-tutorial/blob/main/README.md) [上一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Events%20and%20signals.md) [下一章](https://github.com/LC-space/PyQt6-tutorial/blob/main/Widgets.md)

