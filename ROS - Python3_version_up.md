# ROS - Python 3



## Environment

- Ubuntu 18.04 LTS (Windows 10 듀얼 부팅)
- ROS melodic
- python 2.7

ROS는 기본적으로 python 2.7 버전으로 설치가 된다. 그러나, python 3 버전으로 실행해야 하는 경우가 발생한다. 이후에 진행될 프로젝트들을 위해서 python 3 버전으로 업그레이드 해야할 필요가 있다. 




## Install

아래의 코드 두 줄을 실행한다. 첫번째 줄 코드는 PC와 ROS에 python 3을 설치하는 내용이다. 두번째 줄 코드는 python 3을 설치하는 과정 중에 임의로 손실되는 일부 패키지들을 다시 재설치하는 내용이다. 

```
sudo apt install python3-pip python3-all-dev python3-rospkg
sudo apt install ros-melodic-desktop-full --fix-missing
```




## Change the Version

파이썬 파일을 실행할 때에는 python 명령어와 파이썬 파일 .py를 명령창에 입력한다. 이 때, python 명령어는 기본 실행 버전으로 실행된다. ROS를 설치할 경우, python 2.7 버전으로 실행 됨을 확인할 수 있다. 만약, 기본실행 버전을 그대로 두고 싶다면 python3을 명령어로 입력하면 3 버전으로 실행이 가능하다.


### Check python version

기본 실행 버전을 확인하기 위해서 명령줄에 python이라고 입력을 한다. 그러면 실행되는 버전이 출력되는 것을 확인할 수 있다. python 명령어를 치면 python이 실행됨으로 해당 터미널을 종료하기 위해서는 Alt + D를 입력하면 된다. 

![check python version](https://user-images.githubusercontent.com/91526930/188057211-98228f05-6b44-438b-a085-e582442ed4d4.png)



### Configure python

아래 코드를 실행하면, python 명령어의 기본 실행 버전을 설정할 수 있다. 그러나, 첫 실행 시에는 아래의 화면과 같이 오류 메세지가 나타날 것이다. 

```
sudo update-alternatives --config python
```

![err_msg for config python](https://user-images.githubusercontent.com/91526930/188057624-75a128e9-3fa5-4f58-81da-fb9b052289d8.png)



위의 에러 메시지가 뜰 경우, 파이썬의 두 버전을 아래의 코드와 같이 입력한다. 이후 위의 코드를 다시 입력하면 정상적인 진행을 이어 갈 수 있다.

```
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
```

![update alternatives python](https://user-images.githubusercontent.com/91526930/188057768-95acd6f3-7b42-400f-a9d8-7aee0b2c82bd.png)



정상적으로 실행이 될 경우, 아래와 같은 결과 창과 어떤 파이썬을 기본 실행 버전으로 할 것인지 선택하라는 안내 문구가 출력된다. python 3 버전으로 실행하기 위해서 아래 예시에서는 0으로 입력하였다. 

![config python](https://user-images.githubusercontent.com/91526930/188060408-80174607-4d26-4dac-9636-c5a65f0e1297.png)



파이썬 기본 실행을 하여 확인해보면 다음과 같다.

![check python version](https://user-images.githubusercontent.com/91526930/188060647-0bbe5add-2036-4db9-9f5e-60eeadeb9933.png)




## Run Python Code

python 3버전으로 실행하기 위해서는 파이썬 파일의 첫줄에는 다음의 코드를 추가해야 한다.

```python
#!/usr/bin/env python3
```




## References

https://junk-research-note.tistory.com/10

https://ghostweb.tistory.com/803





## Trouble Shooting

ROS 설치 후 python 3으로 버전업을 완료 한 후, ROS의 전체 코드를 빌드하고 실행하는 경우에 여러 에러 현상이 나타났다. 현재까지 나타난 에러와 그 해결 방법을 공개한다. 에러 제목은 공식명칭이 아닌 본인이 임의로 작성한 것이니 자신의 에러와 키워드가 비슷한 걸 찾으면 될 듯하다.


### 1) Catkin-pkg version error

![Catkin-pkg version error](https://user-images.githubusercontent.com/91526930/188062142-b462e0a1-2423-4a29-90b4-01fbdbb3eb4e.png)

catkin_make를 수행 중 위와 같이 The 'catkin-pkg==0.5.2' distribution was not found 라는 오류 메시지를 발견하였다. catkin-pkg가 사라졌거나, 해당 버전과 다른 버전으로 설치되어 있는 것으로 예상하고 버전에 맞는 pkg로 설치를 진행하도록 한다.

```
pip3 install catkin-pkg==0.5.2
```



### 2) catkin_make 실행시 find_package 에러

![find_package error](https://user-images.githubusercontent.com/91526930/188062465-c4ba8f47-ff9d-4800-98e2-a59bfbdcd05b.png)

위의 오류는 'industrial_robot_client'라는 pkg를 찾지 못했다는 것을 의미한다. 원래 있어야 할 pkg가 파이썬 버전업을 한 이후로 사라진 것으로 추측된다. 위에 보이는 pkg 뿐만이 아니라, ROS 설치시 추가로 설치했던 다른 pkg들도 사라졌다는 것을 어렵지 않게 확인할 수 있었다. 번거롭지만, ROS 설치 시 진행했던 일부 코드를 반복하도록 하자.

```
sudo apt-get install ros-melodic-moveit
sudo apt-get install ros-melodic-industrial-core
sudo apt-get install ros-melodic-joint-state-publisher
sudo apt-get install ros-melodic-trac-ik
sudo apt-get install ros-melodic-moveit-visual-tools
sudo apt-get install ros-melodic-effort-controllers
sudo apt-get install ros-melodic-joint-trajectory-controller
```



### 3) No module named 'defusedxml'

![roslaunch 실행 오류 'defusedxml'](https://user-images.githubusercontent.com/91526930/188063189-4243d425-e65d-47df-8bd5-9f6a306a570f.png)

위 에러는 roslaunch를 실행했으나, 'defusedxml'이라는 모듈을 찾지 못한 것이다. 그러므로, 해당 모듈을 설치하면 된다.

```
sudo apt-get install python3-defusedxml
```



### 4) rospkg version error

![rospkg version error](https://user-images.githubusercontent.com/91526930/188063636-011bba16-2aee-4a0d-afd0-6df136ed844a.png)

위 에러는 roslaunch를 실행했으나, 'rospkg==1.4.0'을 찾지 못한 것이다. 그러므로, 해당 버전의 rospkg를 설치한다.

```
pip3 install rospkg==1.4.0
```









