# 기본 가제보 설치
1. 우분투 24.04에 가제보 설치
-> 이 한 줄로 Gazebo Harmonic + ros_gz_bridge + ros_gz_sim 등 필요한 패키지가 전부 들어오게 된다.
```
sudo apt install ros-jazzy-ros-gz
```

2. 프로젝트에 필요한 추가 패키지 설치
```
sudo apt install \
  ros-jazzy-tf2-ros \
  ros-jazzy-tf2-tools \
  ros-jazzy-robot-state-publisher \
  ros-jazzy-xacro \
  python3-colcon-common-extensions
```

3. 설치 확인
```
source /opt/ros/jazzy/setup.bash

# Gazebo 단독 실행 테스트
gz sim

# ROS2 브릿지 확인
ros2 pkg list | grep gz
```
<img width="1186" height="1028" alt="image" src="https://github.com/user-attachments/assets/6692a083-d6db-4a3b-a406-10c356eccd72" />
<img width="210" height="487" alt="image" src="https://github.com/user-attachments/assets/a7256abb-e221-42f4-bed1-831adebaad39" />

# 프로젝트 실행

