# TrainVideoSimulator

유튜브에는 많은 기차 전면 주행 영상이 있습니다.
이 영상들을 이용하면 기차 시뮬레이션을 할 수 있다고 생각되어 만들어 봤습니다.

프로그램은 setting.txt에 있는 내용을 읽고 시뮬레이션을 합니다.

현재 영상은 cv2의 기본 뷰어로 나타내고 있습니다. pyqt로 작업하고 싶었지만 계속 프리징이 걸리는 문제를 겪고 있습니다. 다른 도구 찾거나 이 문제를 해결하기 까지 시간이 오래 걸릴것 같습니다.

## 알림사항

+ 아직까지 소리가 구현되지 않았습니다.
+ 후진은 아직 구현되지 않았습니다.
+ 물리현상을 적용하지 않았습니다. 그래서 어느 속도든 항상 가속도가 일정합니다.
+ 자세한 신호체계가 구현되지 않았습니다.

## 기본 조작법

w - 가속

s - 감속

ESC - 종료

m - 다음역으로 이동

n - 이전역으로 이동

***

### setting.txt에 대하여

#### videoPath
시뮬레이션에 사용될 동영상 주소입니다. 다운받은 영상이나 유튜브 영상주소를 넣으면 됩니다. 허나 지금은 유튜브영상보다는 영상을 다운받고 구동시키는 것을 추천드립니다.

ex)


    videoPath=Sample.mp4
    또는
    videoPath=https://www.youtube.com/watch?v=~~~~~~
 
 #### mod 
 시뮬레이션을 구동시킬 방법으로 cv2와 pyqt두개를 구현했습니다만 현재 pyqt는 상당한 문제가 있으므로 cv2로 두길 바랍니다.
 
 ex)
 
     mod=cv2
     또는
     mod=pyqt <<<<-비추천

 #### debug
 디버그 모드입니다. True로 할 경우 시뮬레이션전 철도의 속도 그래프와 거리 그래프를 matplotlib.pyplot에서 기본적으로 제공하는 뷰어로 볼 수 있습니다.

ex)

    debug=True
    또는
    debug=False

#### maxSpeed maxAcceleration
동영상속 실제 기차의 최고 속도와 최고 가속도 입니다. 단위는 km/h , k㎨ 입니다

ex)

    maxSpeed=300
    maxAcceleration=1.7
    
#### trainMaxSpeed trainAcceleration trainBreak
시뮬레이션할 열차의 최고속도와 최고 가속도 감속도 입니다. 단위는 km/h , k㎨ 입니다

ex)
    
    trainMaxSpeed=320
    trainAcceleration=1.7
    trainBreak=4
    
 #### driveData
 영상에서 기차가 어떻게 주행했는지 기록하는 부분입니다. 이 밑으로는 반드시 영상내 정보만 기록되어야 합니다.
 영상내 기차 주행 정보는 여러개의 명령어로 구성되어 있습니다.
 
 ##### station <"역 이름"> <도착시간> <출발시간> <감속도> <가속도> <목표속도>
 
 영상내 역 정보를 기록하는 구문 입니다.
 
 역 이름은 반드시 쌍따옴표로 적어야 합니다.
 
 출발시간과 도착시간은 영상내 타임라인 입니다.
 
 감속도는 역에 정차할때 감속도입니다.
 
 가속도는 역에서 출발하고 나서 가속도 입니다.
 
 가속도, 감속도를 0으로 적으면 기존에 설정한 최대 가속도 또는 감속도로 간주합니다.
 
 목표속도는 열차가 역에서 출발하고 지정된 가속도로 가속하고 도달하는 속도입니다.
 
 시간은 mm:ss 또는 hh:mm:ss 형식으로 적어야 합니다.
 
 ex)
 
     station "Cheonan-Asan" 28:12 29:44 1 1.7 300
위의 뜻은 영상의 28:12인 시점에 열차가 1km/s2의 감속도로 천안아산역에 도착하고 29:44시점에 1.7k㎨의 가속도로 출발하고 300km/h로 주행한다 라는 뜻 입니다.

도착시간과 출발시간이 같으면 무정차하는 역으로 간주합니다.
ex)

    station "Cheonan-Asan" 28:12 28:12
위의 뜻은 영상의 열차는 28:12에 천안아산역을 그냥 지나친다 라는 뜻 입니다.

마지막역은 도착시간만 적어도 됩니다.

ex)

    station "Suseo" 1:15:00
위의 뜻은 영상 1:15:00 에 열차가 마지막으로 수서역에 도착합니다.
    
#### acceleration <시간> <가속도> <목표속도>

영상내에 열차가 가속을 시작하는 시점을 기록한 구문입니다.

ex)

    acceleration 03:45 1.7 200
위의 뜻은 영상 03:45부터 1.7의 가속도로 200km/h까지 가속한다는 구문 입니다.

감속은 지원되지 않습니다.

#### speed <시간> <속도> <가속도,감속도>

영상내에 열차가 지정된 속도로 달린는 시점을 기록한 구문입니다.

기록된 시점 이전의 속도차이를 이어주기 위해 가속도 또는 감속도를 기록해야됩니다.

ex)

    speed 39:27 100 1.5
위의 뜻은 열차가 39:27부터 100km/h로 달린다는 뜻입니다. 
그런데 39:27 이전에 속도가 100km/h이상이라면 필요한 시점부터 1.5의 감속도로 감속합니다. 만약 100km/h이하라면 1.5의 가속도로 가속합니다.
    




---
    
## 알려진 문제점

+ pyqt로 실행하면 프리징
+ pyqt로 실행하면 가끔 이유없이 강제종료됨
+ 마지막 역으로 바로 이동하면 강제종료
+ 유튜브 영상이 너무 길면 지나치게 버퍼링이 걸림
+ 시뮬레이션속도가 영상속 실제 속도를 넘어서면 급격하게 느려짐
+ 영어 이외의 언어 안됨

## 노트
+2021-07-30
    +최초 업로드
