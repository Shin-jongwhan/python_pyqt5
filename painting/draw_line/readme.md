### 231009
## 선, 박스를 그리는 방법
### 다음을 참고하였다.
- [위키독스](https://wikidocs.net/74077)
- [pyqt5 - RGB 값 넣기, Qcolor](https://wikidocs.net/37457)
### <br/><br/><br/>

## 예제
### paintEvent(self, e) 함수는 따로 호출하지 않아도 자동적으로 호출된다. 이 함수 안에 페인팅 함수들을 넣어주면 된다.
```
## Ex 8-2. 직선 그리기 (drawLine).

import sys
from PyQt5.QtWidgets import QWidget, QApplication
from PyQt5.QtGui import QPainter, QPen, QColor, QBrush
from PyQt5.QtCore import Qt


class MyApp(QWidget):

    def __init__(self):
        super().__init__()
        self.initUI()

    def initUI(self):
        self.setGeometry(300, 300, 400, 300)
        self.setWindowTitle('drawLine')
        self.show()

    def paintEvent(self, e):
        qp = QPainter()
        qp.begin(self)
        self.draw_line(qp)
        self.draw_line_2(qp)
        self.draw_rect(qp)
        qp.end()


    def draw_line(self, qp):
        qp.setPen(QPen(Qt.blue, 8))
        qp.drawLine(30, 230, 200, 50)
        qp.setPen(QPen(Qt.green, 12))
        qp.drawLine(140, 60, 320, 280)
        qp.setPen(QPen(Qt.red, 16))
        qp.drawLine(330, 250, 40, 190)


    def draw_line_2(self, qp):
        qp.setPen(QPen(Qt.black, 3, Qt.SolidLine))
        qp.drawLine(20, 20, 380, 20)
        qp.drawText(30, 40, 'Qt.SolidLine')

        qp.setPen(QPen(QColor(180, 100, 160, ), 3, Qt.DashLine))
        qp.drawLine(20, 70, 380, 70)
        qp.drawText(30, 90, 'Qt.DashLine')

        qp.setPen(QPen(Qt.black, 3, Qt.DotLine))
        qp.drawLine(20, 120, 380, 120)
        qp.drawText(30, 140, 'Qt.DotLine')

        qp.setPen(QPen(Qt.black, 3, Qt.DashDotLine))
        qp.drawLine(20, 170, 380, 170)
        qp.drawText(30, 190, 'Qt.DashDotLine')

        qp.setPen(QPen(Qt.black, 3, Qt.DashDotDotLine))
        qp.drawLine(20, 220, 380, 220)
        qp.drawText(30, 240, 'Qt.DashDotDotLine')

        pen = QPen(Qt.black, 3, Qt.CustomDashLine)
        pen.setDashPattern([4, 3, 2, 5])
        qp.setPen(pen)
        qp.drawLine(20, 270, 380, 270)
        qp.drawText(30, 290, 'Qt.CustomDashLine')


    def draw_rect(self, qp):
        qp.setBrush(QColor(180, 100, 160))
        qp.setPen(QPen(QColor(60, 60, 60), 3))
        qp.drawRect(20, 20, 100, 100)

        qp.setBrush(QColor(40, 150, 20))
        qp.setPen(QPen(Qt.blue, 2))
        qp.drawRect(180, 120, 50, 120)

        qp.setBrush(Qt.yellow)
        qp.setPen(QPen(Qt.red, 5))
        qp.drawRect(280, 30, 80, 40)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())

```
### 이렇게 나온다.
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/4502e105-c02f-4977-ba2b-32063fc03cb2)
### <br/>

## 함수 일반화
### Qt_cofig.py
### 다음과 같이 Qt 에 대한 config 스크립트를 작성해둔다. 여기서 컬러나 라인 스타일을 불러와준다.
```
from PyQt5 import QtWidgets
from PyQt5 import QtCore
from PyQt5.QtGui import QPainter, QPen, QColor, QBrush


class GUI : 
    def __init__(self) : 
        self.dicQt_global_color = {
            "black" : QtCore.Qt.black, 
            "white" : QtCore.Qt.white, 
            "darkGray" : QtCore.Qt.darkGray, 
            "gray" : QtCore.Qt.gray, 
            "lightGray" : QtCore.Qt.lightGray, 
            "red" : QtCore.Qt.red, 
            "green" : QtCore.Qt.green, 
            "blue" : QtCore.Qt.blue, 
            "cyan" : QtCore.Qt.cyan, 
            "magenta" : QtCore.Qt.magenta, 
            "yellow" : QtCore.Qt.yellow, 
            "darkRed" : QtCore.Qt.darkRed, 
            "darkGreen" : QtCore.Qt.darkGreen, 
            "darkBlue" : QtCore.Qt.darkBlue, 
            "darkCyan" : QtCore.Qt.darkCyan, 
            "darkMagenta" : QtCore.Qt.darkMagenta, 
            "darkYellow" : QtCore.Qt.darkYellow, 
            "transparent" : QtCore.Qt.transparent
        }
        self.dicLine_style = {
            "NoPen" : QtCore.Qt.NoPen, 
            "SolidLine" : QtCore.Qt.SolidLine, 
            "DashLine" : QtCore.Qt.DashLine, 
            "DotLine" : QtCore.Qt.DotLine, 
            "DashDotLine" : QtCore.Qt.DashDotLine, 
            "DashDotDotLine" : QtCore.Qt.DashDotDotLine, 
            "CustomDashLine" : QtCore.Qt.CustomDashLine, 
            "MPenStyle" : QtCore.Qt.MPenStyle
        }
```
### 
```
import sys
from PyQt5.QtWidgets import QWidget, QApplication
from PyQt5.QtGui import QPainter, QPen, QColor, QBrush
from PyQt5.QtCore import Qt

import Qt_config


class MyApp(QWidget):

    def __init__(self):
        super().__init__()
        self.Qt_config = Qt_config.GUI()
        self.initUI()

    def initUI(self):
        self.setGeometry(300, 300, 400, 300)
        self.setWindowTitle('drawLine')
        self.show()

    def paintEvent(self, e):
        qp = QPainter()
        qp.begin(self)
        self.draw_line(qp, 30, 230, 200, 50, "blue", 10)
        qp.end()


    def draw_line(self, qp, x1, y1, x2, y2, color, nLine_size, sLine_style = "SolidLine") :
        # color
        ## string : if you give color to string, then you will use global color
        ## list : if you give color to [red, green, blue, alpha], then you will use RGB code(0 ~ 255)
        if type(color) == str : 
            if color not in self.Qt_config.dicQt_global_color.keys() : 
                print("There's no color for {0}".format(color))
                return
            qp.setPen(QPen(self.Qt_config.dicQt_global_color[color], nLine_size))
            qp.drawLine(x1, y1, x2, y2)
        elif type(color) == list : 
            if len(color) != 4 and all(list((map(lambda x : type(x) == int, color)))) :         # 길이가 4이고 모두 정수형인지 체크
                print("You should set color paramer to [red, green, blue, alpha] within ragne between 0 and 255.")
                return
            qp.setPen(QPen(QColor(color), 3, self.Qt_config.dicLine_style[sLine_style]))
            qp.drawLine(x1, y1, x2, y2)


if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = MyApp()
    sys.exit(app.exec_())
```
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/2de987be-f738-4ecf-8bc9-f43b9207e292)




