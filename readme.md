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


## example
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
### <br/><br/><br/>

## 버튼을 편하게 만드는 방법
### create_button() 함수와 같이 버튼을 생성하는 함수를 하나 만들어둔다.
```
import sys
from PyQt5 import QtWidgets
from PyQt5 import QtCore

import kiwoom


class Main(QtWidgets.QMainWindow):
    def __init__(self):
        super().__init__()
        print("메인 클래스입니다.")
        self.event_loop = QtCore.QEventLoop()
        self.kiwoom = kiwoom.kiwoom()

        self.create_button("키움 API 로그인", 10, 10, 100, 30, self.kiwoom.connect)
        self.create_button("주식일봉차트 - rq", 10, 500, 200, 30, lambda : self.kiwoom.opt10081_rq("005930", "20230930", 0))
        self.create_button("OnReceiveTrData 조회 등록", 10, 160, 200, 30, self.kiwoom.OnReceiveTrData_conn)
        self.create_button("주식일봉차트 조회 등록", 10, 190, 200, 30, self.kiwoom.opt10081_conn)
        self.create_button("데이터 조회 개수", 10, 220, 200, 30, self.kiwoom.get_data_index)

        
    def create_button(self, btn_value, move_x, move_y, size_w, size_h, func) : 
        btn = QtWidgets.QPushButton(btn_value, self)
        btn.move(move_x, move_y)
        btn.resize(size_w, size_h)
        btn.clicked.connect(func)


if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    window = Main()
    window.show()
    window.setGeometry(200, 200, 800, 600)
    app.exec_() # 이벤트 루프 실행.
```
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/fb2e4449-aef5-4f71-8e45-c6b84550e59f)
### <br/><br/><br/>


## PyQt5.QtAxContainer.QAxWidget
### QAxWidget 은 다른 프로그램을 연결하는 모듈이다.
### 컨트롤러 연결 방법
```
self.ocx = QAxContainer.QAxWidget("A1574A0D-6BFA-4BD7-9020-DED88711818D")        # 키움 API 공식 가이드에 나와 있는 control 레지스트리 값

```
### 이렇게 다른 프로그램과 연결할 수 있다.
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/dc043045-e19c-479b-9707-eacff1b29549)
### <br/>

### 함수 연결 방법
```
self.ocx.dynamicCall("SetInputValue(sID, sValue)", id, value)
```
### connect 함수로 여러 함수를 연결하면 request (주식일봉차트 - rq) 를 누를 때마다 모든 connected 된 함수를 실행한다.
### 같은 함수도 여러 번 등록하면 여러 번 실행된다.
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/f28baf38-b6ee-4774-ac05-11bdb5b880f1)
### <br/>

### 연결된 함수 초기화 방법
### 함수가 여러 번 실행되지 않고 한 번만 실행되게 하려면 초기화를 진행해주어야 한다.
```
    def clear_conn_func(self) : 
        try : 
            self.ocx.disconnect()
        except : 
            print("Nothing to clear")
```
### 클리어를 눌러주고 다시 함수 연결을 하면 재등록된 함수만 실행 된다.
#### ![image](https://github.com/Shin-jongwhan/python_pyqt5/assets/62974484/17b93e7c-8291-4a55-b879-81f11d5b3a73)
### <br/><br/><br/>






