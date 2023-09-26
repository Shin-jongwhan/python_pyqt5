### 230926
## GUI 만드는 PyQt5 라이브러리 사용법을 정리하는 공간
#### * PyQt5 는 굉장히 큰 라이브러리이고, 책 학권이 있을 정도이다.
### 참고 사이트
- https://wikidocs.net/90850
### <br/><br/><br/>


## 개념
### exec_() : 버튼이 클릭이 되면 함수를 실행하는데, exec_() 앞쪽에 있으면 그 코드를 실행하고 있고, 뒤쪽에 있으면 코드 실행하는 것을 홀드시킨다.
### 언제 사용하는지?
- 앞의 작업이 모두 완료되는 것을 기다려야 할 때
- 프로그램이 계속 살아있게 만들어야할 때(예를 들어 자동매매 프로그램을 만들었는데, 계속 무한루프로 실행하고 있어야 할 때)
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/d63bb2e9-e1ad-4726-9bbd-d2f59dd781df)
### <br/><br/><br/>


## 예제 1
### QtWidgets.QPushButton 으로 버튼을 생성할 수 있다.
### btn.clicked.connect 로 실행할 함수를 연결한다.
### btn4.clicked.connect(lambda : self.btn4_clicked("test")) 와 같이 parameter 를 주고 싶을 때는 lambda 를 이용한다.
### window.setGeometry(200, 200, 800, 600) : 왼쪽 위 모서리 기준으로, 화면 x, y - 200, 200 위치에 띄우고 창 크기 width, height 를 800, 600 크기로 띄운다.
### self.event_loop.exec_() : 앞의 코드 실행하기까지를 기다린다.
### self.event_loop.exit() : self.event_loop.exec_() 에서 기다린 코드를 실행한다.
```
import sys
from PyQt5 import QtWidgets
from PyQt5 import QtCore
import time


class MyWindow(QtWidgets.QMainWindow):
    def __init__(self):
        super().__init__()

        self.event_loop = QtCore.QEventLoop()

        btn1 = QtWidgets.QPushButton("button1", self)
        btn1.move(10, 10)
        btn1.clicked.connect(self.btn1_clicked)

        btn2 = QtWidgets.QPushButton("button2", self)
        btn2.move(10, 40)
        btn2.clicked.connect(self.btn2_clicked)

        btn3 = QtWidgets.QPushButton("button3", self)
        btn3.move(10, 70)
        btn3.clicked.connect(self.btn3_clicked)

        btn4 = QtWidgets.QPushButton("button4", self)
        btn4.move(10, 100)
        btn4.clicked.connect(lambda : self.btn4_clicked("test"))        # lambda 함수로 argument 를 줄 수 있다.

    def btn1_clicked(self):
        print("before loop exec")
        self.event_loop.exec_()
        print("after loop exec")

    def btn2_clicked(self):
        print("before loop exit")
        self.event_loop.exit()
        print("after loop exit")
        time.sleep(5)
        print("after time sleep")

    def btn3_clicked(self):
        print("button3 clicked event")


    def btn4_clicked(self, a):
        print("before loop exec")
        print(a)
        self.event_loop.exec_()
        print("after loop exec")


if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    window = MyWindow()
    window.setGeometry(200, 200, 800, 600)
    window.show()
    app.exec_()
```
### 아래와 같이 버튼을 생성한다.
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/8f10b2d1-f628-49dc-91b0-91ba3a15c82a)
### 버튼 1 -> 버튼 2 순서로 누르면 이렇게 실행된다.
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/cc20e1e6-74be-4515-80ef-2303dd41624e)
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/1fde8001-7b4e-4105-b272-a1a702ac7cb2)


