# TrainVideoSimulator

유튜브에는 많은 기차 전면 주행 영상이 있습니다.
이 영상들을 이용하면 기차 시뮬레이션을 할 수 있다고 생각되어 만들어 봤습니다.

프로그램은 setting.txt에 있는 내용을 읽고 시뮬레이션을 합니다.

### 기본 조작법

w - 가속

s - 감속

ESC - 종료

m - 다음역으로 이동

n - 이전역으로 이동

### setting.txt에 대하여

videoPath - 시뮬레이션에 사용될 동영상 주소입니다. 다운받은 영상이나 유튜브 영상주소를 넣으면 됩니다


    videoPath=Sample.mp4
    또는
    videoPath=https://www.youtube.com/watch?v=~~~~~~
 
 mod - 시뮬레이션을 구동시킬 방법으로 cv2와 pyqt두개를 구현했습니다만 현재 pyqt는 상당한 문제가 있으므로 cv2로 두길 바랍니다.
 
     mod=cv2
     또는
     mod=pyqt <<<<-비추천

debug - 디버그 모드입니다. True로 할 경우 시뮬레이션전 철도의 속도 그래프와 거리 그래프를 볼 수 있습니다.

    debug=True
    또는
    debug=False

## 알려진 문제점

+ pyqt로 실행하면 프리징
+ pyqt로 실행하면 가끔 이유없이 강제종료됨
+ 마지막 역으로 바로 이동하면 강제종료
+ 유튜브 영상이 너무 길면 지나치게 버퍼링이 걸림
+ 시뮬레이션속도가 영상속 실제 속도를 넘어서면 급격하게 느려짐
+ 영어 이외의 언어 안됨
