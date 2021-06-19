[目录]() [上一章]()

# PyQt6俄罗斯方块

*最近更新于2021年5月5日*

在本章中，我们将创造一款《俄罗斯方块》的克隆游戏。

## 俄罗斯方块

俄罗斯方块游戏是有史以来最受欢迎的电脑游戏之一。最初的游戏是由俄罗斯程序员Alexey Pajitnov在1985年设计和编写的。从那时起，《俄罗斯方块》便以各种形式出现在几乎所有电脑平台上。

俄罗斯方块是一种下落的方块拼图游戏。在这个游戏中，我们有七种不同的形状，叫做Tetrominoe：S形、Z形、T形、L形、线形、镜像L形和方形。每一个形状都由四个正方形组成。这些形状正从Board上掉下来。俄罗斯方块游戏的目标是移动和旋转形状，使它们尽可能适合。如果我们能排成一排，那排就会被破坏，我们就能得分。当方块从顶部溢出时结束游戏。

![Tetrominoes](F:\programming\Cplus\Qt\PyQt6-tutorial\The Tetris game\img\tetrominoes.png)

PyQt6是一个用于创建应用程序的工具包。还有其他一些库是专门用来制作电脑游戏的。不过，PyQt6和其他应用程序工具包可以用来创建简单的游戏。

制作电脑游戏是提高编程技能的好方法。

## 开发

我们没有俄罗斯方块游戏的图像，我们使用PyQt6编程工具包中可用的绘图API绘制了俄罗斯方块。每个电脑游戏背后都有一个数学模型。《俄罗斯方块》就是这样。

游戏背后的一些理念：

* 我们使用QtCore。QBasicTimer来创建一个游戏循环。
* Tetrominoe是画出来的。
* 形状在一个正方形的基础上移动（不是一个像素一个像素）。
* 从数学上讲，Board就是一个简单的数字列表。

该代码包括四类：Tetris、Board、Tetrominoe和Shape。Tetris类设置游戏。Board是编写游戏逻辑的地方。Tetrominoe类包含所有俄罗斯方块的名称，Shape类包含一个俄罗斯方块的代码。

```python
# tetris.py
#!/usr/bin/python

"""
ZetCode PyQt6 tutorial

This is a Tetris game clone.

Author: Jan Bodnar
Website: zetcode.com
"""

import random
import sys

from PyQt6.QtCore import Qt, QBasicTimer, pyqtSignal
from PyQt6.QtGui import QPainter, QColor
from PyQt6.QtWidgets import QMainWindow, QFrame, QApplication


class Tetris(QMainWindow):

    def __init__(self):
        super().__init__()

        self.initUI()


    def initUI(self):
        """initiates application UI"""

        self.tboard = Board(self)
        self.setCentralWidget(self.tboard)

        self.statusbar = self.statusBar()
        self.tboard.msg2Statusbar[str].connect(self.statusbar.showMessage)

        self.tboard.start()

        self.resize(180, 380)
        self.center()
        self.setWindowTitle('Tetris')
        self.show()


    def center(self):
        """centers the window on the screen"""

        qr = self.frameGeometry()
        cp = self.screen().availableGeometry().center()

        qr.moveCenter(cp)
        self.move(qr.topLeft())


class Board(QFrame):

    msg2Statusbar = pyqtSignal(str)

    BoardWidth = 10
    BoardHeight = 22
    Speed = 300


    def __init__(self, parent):
        super().__init__(parent)

        self.initBoard()


    def initBoard(self):
        """initiates board"""

        self.timer = QBasicTimer()
        self.isWaitingAfterLine = False

        self.curX = 0
        self.curY = 0
        self.numLinesRemoved = 0
        self.board = []

        self.setFocusPolicy(Qt.FocusPolicy.StrongFocus)
        self.isStarted = False
        self.isPaused = False
        self.clearBoard()


    def shapeAt(self, x, y):
        """determines shape at the board position"""

        return self.board[(y * Board.BoardWidth) + x]


    def setShapeAt(self, x, y, shape):
        """sets a shape at the board"""

        self.board[(y * Board.BoardWidth) + x] = shape


    def squareWidth(self):
        """returns the width of one square"""

        return self.contentsRect().width() // Board.BoardWidth


    def squareHeight(self):
        """returns the height of one square"""

        return self.contentsRect().height() // Board.BoardHeight


    def start(self):
        """starts game"""

        if self.isPaused:
            return

        self.isStarted = True
        self.isWaitingAfterLine = False
        self.numLinesRemoved = 0
        self.clearBoard()

        self.msg2Statusbar.emit(str(self.numLinesRemoved))

        self.newPiece()
        self.timer.start(Board.Speed, self)


    def pause(self):
        """pauses game"""

        if not self.isStarted:
            return

        self.isPaused = not self.isPaused

        if self.isPaused:
            self.timer.stop()
            self.msg2Statusbar.emit("paused")

        else:
            self.timer.start(Board.Speed, self)
            self.msg2Statusbar.emit(str(self.numLinesRemoved))

        self.update()


    def paintEvent(self, event):
        """paints all shapes of the game"""

        painter = QPainter(self)
        rect = self.contentsRect()

        boardTop = rect.bottom() - Board.BoardHeight * self.squareHeight()

        for i in range(Board.BoardHeight):
            for j in range(Board.BoardWidth):
                shape = self.shapeAt(j, Board.BoardHeight - i - 1)

                if shape != Tetrominoe.NoShape:
                    self.drawSquare(painter,
                                    rect.left() + j * self.squareWidth(),
                                    boardTop + i * self.squareHeight(), shape)

        if self.curPiece.shape() != Tetrominoe.NoShape:

            for i in range(4):
                x = self.curX + self.curPiece.x(i)
                y = self.curY - self.curPiece.y(i)
                self.drawSquare(painter, rect.left() + x * self.squareWidth(),
                            boardTop + (Board.BoardHeight - y - 1) * self.squareHeight(),
                            self.curPiece.shape())


    def keyPressEvent(self, event):
        """processes key press events"""

        if not self.isStarted or self.curPiece.shape() == Tetrominoe.NoShape:
            super(Board, self).keyPressEvent(event)
            return

        key = event.key()

        if key == Qt.Key.Key_P:
            self.pause()
            return

        if self.isPaused:
            return

        elif key == Qt.Key.Key_Left.value:
            self.tryMove(self.curPiece, self.curX - 1, self.curY)

        elif key == Qt.Key.Key_Right.value:
            self.tryMove(self.curPiece, self.curX + 1, self.curY)

        elif key == Qt.Key.Key_Down.value:
            self.tryMove(self.curPiece.rotateRight(), self.curX, self.curY)

        elif key == Qt.Key.Key_Up.value:
            self.tryMove(self.curPiece.rotateLeft(), self.curX, self.curY)

        elif key == Qt.Key.Key_Space.value:
            self.dropDown()

        elif key == Qt.Key.Key_D.value:
            self.oneLineDown()

        else:
            super(Board, self).keyPressEvent(event)


    def timerEvent(self, event):
        """handles timer event"""

        if event.timerId() == self.timer.timerId():

            if self.isWaitingAfterLine:
                self.isWaitingAfterLine = False
                self.newPiece()
            else:
                self.oneLineDown()

        else:
            super(Board, self).timerEvent(event)


    def clearBoard(self):
        """clears shapes from the board"""

        for i in range(Board.BoardHeight * Board.BoardWidth):
            self.board.append(Tetrominoe.NoShape)


    def dropDown(self):
        """drops down a shape"""

        newY = self.curY

        while newY > 0:

            if not self.tryMove(self.curPiece, self.curX, newY - 1):
                break

            newY -= 1

        self.pieceDropped()


    def oneLineDown(self):
        """goes one line down with a shape"""

        if not self.tryMove(self.curPiece, self.curX, self.curY - 1):
            self.pieceDropped()


    def pieceDropped(self):
        """after dropping shape, remove full lines and create new shape"""

        for i in range(4):
            x = self.curX + self.curPiece.x(i)
            y = self.curY - self.curPiece.y(i)
            self.setShapeAt(x, y, self.curPiece.shape())

        self.removeFullLines()

        if not self.isWaitingAfterLine:
            self.newPiece()


    def removeFullLines(self):
        """removes all full lines from the board"""

        numFullLines = 0
        rowsToRemove = []

        for i in range(Board.BoardHeight):

            n = 0
            for j in range(Board.BoardWidth):
                if not self.shapeAt(j, i) == Tetrominoe.NoShape:
                    n = n + 1

            if n == 10:
                rowsToRemove.append(i)

        rowsToRemove.reverse()

        for m in rowsToRemove:

            for k in range(m, Board.BoardHeight):
                for l in range(Board.BoardWidth):
                    self.setShapeAt(l, k, self.shapeAt(l, k + 1))

        numFullLines = numFullLines + len(rowsToRemove)

        if numFullLines > 0:
            self.numLinesRemoved = self.numLinesRemoved + numFullLines
            self.msg2Statusbar.emit(str(self.numLinesRemoved))

            self.isWaitingAfterLine = True
            self.curPiece.setShape(Tetrominoe.NoShape)
            self.update()


    def newPiece(self):
        """creates a new shape"""

        self.curPiece = Shape()
        self.curPiece.setRandomShape()
        self.curX = Board.BoardWidth // 2 + 1
        self.curY = Board.BoardHeight - 1 + self.curPiece.minY()

        if not self.tryMove(self.curPiece, self.curX, self.curY):

            self.curPiece.setShape(Tetrominoe.NoShape)
            self.timer.stop()
            self.isStarted = False
            self.msg2Statusbar.emit("Game over")


    def tryMove(self, newPiece, newX, newY):
        """tries to move a shape"""

        for i in range(4):

            x = newX + newPiece.x(i)
            y = newY - newPiece.y(i)

            if x < 0 or x >= Board.BoardWidth or y < 0 or y >= Board.BoardHeight:
                return False

            if self.shapeAt(x, y) != Tetrominoe.NoShape:
                return False

        self.curPiece = newPiece
        self.curX = newX
        self.curY = newY
        self.update()

        return True


    def drawSquare(self, painter, x, y, shape):
        """draws a square of a shape"""

        colorTable = [0x000000, 0xCC6666, 0x66CC66, 0x6666CC,
                      0xCCCC66, 0xCC66CC, 0x66CCCC, 0xDAAA00]

        color = QColor(colorTable[shape])
        painter.fillRect(x + 1, y + 1, self.squareWidth() - 2,
                         self.squareHeight() - 2, color)

        painter.setPen(color.lighter())
        painter.drawLine(x, y + self.squareHeight() - 1, x, y)
        painter.drawLine(x, y, x + self.squareWidth() - 1, y)

        painter.setPen(color.darker())
        painter.drawLine(x + 1, y + self.squareHeight() - 1,
                         x + self.squareWidth() - 1, y + self.squareHeight() - 1)
        painter.drawLine(x + self.squareWidth() - 1,
                         y + self.squareHeight() - 1, x + self.squareWidth() - 1, y + 1)


class Tetrominoe:

    NoShape = 0
    ZShape = 1
    SShape = 2
    LineShape = 3
    TShape = 4
    SquareShape = 5
    LShape = 6
    MirroredLShape = 7


class Shape:

    coordsTable = (
        ((0, 0), (0, 0), (0, 0), (0, 0)),
        ((0, -1), (0, 0), (-1, 0), (-1, 1)),
        ((0, -1), (0, 0), (1, 0), (1, 1)),
        ((0, -1), (0, 0), (0, 1), (0, 2)),
        ((-1, 0), (0, 0), (1, 0), (0, 1)),
        ((0, 0), (1, 0), (0, 1), (1, 1)),
        ((-1, -1), (0, -1), (0, 0), (0, 1)),
        ((1, -1), (0, -1), (0, 0), (0, 1))
    )

    def __init__(self):

        self.coords = [[0, 0] for i in range(4)]
        self.pieceShape = Tetrominoe.NoShape

        self.setShape(Tetrominoe.NoShape)


    def shape(self):
        """returns shape"""

        return self.pieceShape


    def setShape(self, shape):
        """sets a shape"""

        table = Shape.coordsTable[shape]

        for i in range(4):
            for j in range(2):
                self.coords[i][j] = table[i][j]

        self.pieceShape = shape


    def setRandomShape(self):
        """chooses a random shape"""

        self.setShape(random.randint(1, 7))


    def x(self, index):
        """returns x coordinate"""

        return self.coords[index][0]


    def y(self, index):
        """returns y coordinate"""

        return self.coords[index][1]


    def setX(self, index, x):
        """sets x coordinate"""

        self.coords[index][0] = x


    def setY(self, index, y):
        """sets y coordinate"""

        self.coords[index][1] = y
        

    def minX(self):
        """returns min x value"""

        m = self.coords[0][0]
        for i in range(4):
            m = min(m, self.coords[i][0])

        return m


    def maxX(self):
        """returns max x value"""

        m = self.coords[0][0]
        for i in range(4):
            m = max(m, self.coords[i][0])

        return m


    def minY(self):
        """returns min y value"""

        m = self.coords[0][1]
        for i in range(4):
            m = min(m, self.coords[i][1])

        return m


    def maxY(self):
        """returns max y value"""

        m = self.coords[0][1]
        for i in range(4):
            m = max(m, self.coords[i][1])

        return m


    def rotateLeft(self):
        """rotates shape to the left"""

        if self.pieceShape == Tetrominoe.SquareShape:
            return self

        result = Shape()
        result.pieceShape = self.pieceShape

        for i in range(4):
            result.setX(i, self.y(i))
            result.setY(i, -self.x(i))

        return result


    def rotateRight(self):
        """rotates shape to the right"""

        if self.pieceShape == Tetrominoe.SquareShape:
            return self

        result = Shape()
        result.pieceShape = self.pieceShape

        for i in range(4):
            result.setX(i, -self.y(i))
            result.setY(i, self.x(i))

        return result


def main():

    app = QApplication([])
    tetris = Tetris()
    sys.exit(app.exec())


if __name__ == '__main__':
    main()
```

游戏稍微简化了一点，以便更容易理解。游戏启动后立即开始。我们可以按P键暂停游戏。空格键会立即将方块掉落到底部。游戏以恒定的速度运行，没有加速。分数是我们已经删除的行数。

```python
self.tboard = Board(self)
self.setCentralWidget(self.tboard)
```

创建Board类的一个实例，并将其设置为应用程序的中心控件。

```python
self.statusbar = self.statusBar()
self.tboard.msg2Statusbar[str].connect(self.statusbar.showMessage)
```

我们创建一个显示消息的状态栏。我们将显示三个可能的消息：已删除的行数、暂停的消息或游戏结束消息。msg2Statusbar是在Board类中实现的自定义信号。showMessage是一个内置的方法，它在状态栏上显示消息。

```python
self.tboard.start()
```

这一行开始游戏。

```python
class Board(QFrame):

    msg2Statusbar = pyqtSignal(str)
...
```

使用pyqtSignal创建自定义信号。msg2Statusbar是当我们想向状态栏写入消息或分数时发出的信号。

```python
BoardWidth = 10
BoardHeight = 22
Speed = 300
```

这些是Board的类变量。BoardWidth和BoardHeight以块的形式定义board的大小。速度定义了游戏的速度。每300毫秒一个新的游戏周期将开始。

```python
...
self.curX = 0
self.curY = 0
self.numLinesRemoved = 0
self.board = []
...
```

在initBoard方法中，我们初始化了一些重要的变量。self.board变量是一个从0到7的数字列表。它表示各种形状的位置和形状在board上的剩余位置。

```python
def shapeAt(self, x, y):
    """determines shape at the board position"""

    return self.board[(y * Board.BoardWidth) + x]
```

shapeAt方法确定给定形状块的类型。

```python
def squareWidth(self):
    """returns the width of one square"""

    return self.contentsRect().width() // Board.BoardWidth
```

可以动态调整单board的大小。因此，块的大小可能会改变。squareWidth以像素为单位计算单个正方形的宽度并返回它。Board.BoardWidth是board的大小。

```python
def pause(self):
    """pauses game"""

    if not self.isStarted:
        return

    self.isPaused = not self.isPaused

    if self.isPaused:
        self.timer.stop()
        self.msg2Statusbar.emit("paused")

    else:
        self.timer.start(Board.Speed, self)
        self.msg2Statusbar.emit(str(self.numLinesRemoved))

    self.update()
```

pause方法暂停游戏。它会停止计时器并在状态栏上显示一条消息。

```python
def paintEvent(self, event):
    """paints all shapes of the game"""

    painter = QPainter(self)
    rect = self.contentsRect()
...
```

绘图发生在paintEvent方法中。QPainter负责PyQt6中所有低级的绘图。

```python
for i in range(Board.BoardHeight):
    for j in range(Board.BoardWidth):
        shape = self.shapeAt(j, Board.BoardHeight - i - 1)

        if shape != Tetrominoe.NoShape:
            self.drawSquare(painter,
                rect.left() + j * self.squareWidth(),
                boardTop + i * self.squareHeight(), shape)
```

游戏的绘图分为两个步骤。在第一步中，我们绘制所有的形状，或已被放置到board底部的形状的剩余部分。所有的方块都记在self.board列表变量里。使用shapeAt方法访问该变量。

```python
if self.curPiece.shape() != Tetrominoe.NoShape:

    for i in range(4):

        x = self.curX + self.curPiece.x(i)
        y = self.curY - self.curPiece.y(i)
        self.drawSquare(painter, rect.left() + x * self.squareWidth(),
            boardTop + (Board.BoardHeight - y - 1) * self.squareHeight(),
            self.curPiece.shape())
```

下一步是画出正在掉落的方块。

```python
elif key == Qt.Key.Key_Right.value:
    self.tryMove(self.curPiece, self.curX + 1, self.curY)
```

在keyPressEvent方法中，我们检查是否按下了键。如果我们按下右箭头键，我们就会尝试向右移动方块。我们说尝试，是因为方块可能无法移动。

```python
elif key == Qt.Key.Key_Up.value:
    self.tryMove(self.curPiece.rotateLeft(), self.curX, self.curY)
```

向上的方向键将把下落的方块向左旋转。

```python
elif key == Qt.Key.Key_Space.value:
    self.dropDown()
```

空格键会立即将下落的方块掉落到底部。

```python
elif key == Qt.Key.Key_D.value:
    self.oneLineDown()
```

按下D键，方块就会向下走一段。它可以用来加速方块的下落。

```python
def timerEvent(self, event):
    """handles timer event"""

    if event.timerId() == self.timer.timerId():

        if self.isWaitingAfterLine:
            self.isWaitingAfterLine = False
            self.newPiece()
        else:
            self.oneLineDown()

    else:
        super(Board, self).timerEvent(event)
```

在timer事件中，我们要么在前一个掉落到底部之后创建一个新方块，要么将底部的方块向下移动一行。

```python
def clearBoard(self):
    """clears shapes from the board"""

    for i in range(Board.BoardHeight * Board.BoardWidth):
        self.board.append(Tetrominoe.NoShape)
```

clearBoard方法通过在board的每个块上设置Tetrominoe.NoShape来清除board。

```python
def removeFullLines(self):
    """removes all full lines from the board"""

    numFullLines = 0
    rowsToRemove = []

    for i in range(Board.BoardHeight):

        n = 0
        for j in range(Board.BoardWidth):
            if not self.shapeAt(j, i) == Tetrominoe.NoShape:
                n = n + 1

        if n == 10:
            rowsToRemove.append(i)

    rowsToRemove.reverse()


    for m in rowsToRemove:

        for k in range(m, Board.BoardHeight):
            for l in range(Board.BoardWidth):
                    self.setShapeAt(l, k, self.shapeAt(l, k + 1))

    numFullLines = numFullLines + len(rowsToRemove)
 ...
```

如果方块到达底部，我们调用removeFullLines方法。我们找出所有的完整的行，并删除它们。我们通过移动当前整行之上的所有行来做到这一点，并向下删除一行。注意，我们颠倒了要删除的行的顺序。否则，它将不能正常工作。在我们的例子中，我们用的是自然重力。这意味着方块可能漂浮在空白间隙之上。

```python
def newPiece(self):
    """creates a new shape"""

    self.curPiece = Shape()
    self.curPiece.setRandomShape()
    self.curX = Board.BoardWidth // 2 + 1
    self.curY = Board.BoardHeight - 1 + self.curPiece.minY()

    if not self.tryMove(self.curPiece, self.curX, self.curY):

        self.curPiece.setShape(Tetrominoe.NoShape)
        self.timer.stop()
        self.isStarted = False
        self.msg2Statusbar.emit("Game over")
```

newPiece方法会随机创建一个新的俄罗斯方块。如果方块不能进入它的初始位置，游戏就结束了。

```python
def tryMove(self, newPiece, newX, newY):
    """tries to move a shape"""

    for i in range(4):

        x = newX + newPiece.x(i)
        y = newY - newPiece.y(i)

        if x < 0 or x >= Board.BoardWidth or y < 0 or y >= Board.BoardHeight:
            return False

        if self.shapeAt(x, y) != Tetrominoe.NoShape:
            return False

    self.curPiece = newPiece
    self.curX = newX
    self.curY = newY
    self.update()

    return True
```

在tryMove方法中，我们尝试移动形状。如果该形状位于棋盘的边缘或与其他方块相邻，则返回False。否则，我们将当前下落的方块放置到一个新的位置。

```python
class Tetrominoe:

    NoShape = 0
    ZShape = 1
    SShape = 2
    LineShape = 3
    TShape = 4
    SquareShape = 5
    LShape = 6
    MirroredLShape = 7
```

Tetrominoe类包含所有可能形状的名称。对于空形状，我们也有一个NoShape。

Shape类保存关于俄罗斯方块的信息。

```python
class Shape(object):

    coordsTable = (
        ((0, 0),     (0, 0),     (0, 0),     (0, 0)),
        ((0, -1),    (0, 0),     (-1, 0),    (-1, 1)),
        ...
    )
...
```

coordsTable元组保存了俄罗斯方块所有可能的坐标值。这是一个模板，所有的部分都从这个模板中获得它们的坐标值。

```python
self.coords = [[0,0] for i in range(4)]
```

在创建之后，我们创建了一个空的坐标列表。该列表将保存俄罗斯方块的坐标。

![Coordinates](F:\programming\Cplus\Qt\PyQt6-tutorial\The Tetris game\img\coordinates.png)

上面的图像将有助于理解更多的坐标值。例如，元组(0，-1)、(0,0)、(-1,0)、(-1，-1)表示一个Z形。图表说明了形状。

```python
def rotateLeft(self):
    """rotates shape to the left"""

    if self.pieceShape == Tetrominoe.SquareShape:
        return self

    result = Shape()
    result.pieceShape = self.pieceShape

    for i in range(4):

        result.setX(i, self.y(i))
        result.setY(i, -self.x(i))

    return result
```

rotatleft方法将一个方块向左旋转。正方形不需要旋转。这就是为什么我们只返回对当前对象的引用。创建一个新方块，并将其坐标设置为旋转后的方块的坐标。

![image-20210619231356476](F:\programming\Cplus\Qt\PyQt6-tutorial\The Tetris game\img\image-20210619231356476.png)

这是PyQt6中的俄罗斯方块游戏。