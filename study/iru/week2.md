# 1. 기본 Gazebo 설치 및 실행
### 1. 우분투 24.04에 가제보 설치
-> 이 한 줄로 Gazebo Harmonic + ros_gz_bridge + ros_gz_sim 등 필요한 패키지가 전부 들어오게 된다.
```
sudo apt install ros-jazzy-ros-gz
```

### 2. 프로젝트에 필요한 추가 패키지 설치
```
sudo apt install \
  ros-jazzy-tf2-ros \
  ros-jazzy-tf2-tools \
  ros-jazzy-robot-state-publisher \
  ros-jazzy-xacro \
  python3-colcon-common-extensions
```

### 3. 설치 확인
```
source /opt/ros/jazzy/setup.bash

# Gazebo 단독 실행 테스트
gz sim

# ROS2 브릿지 확인
ros2 pkg list | grep gz
```
<img width="1186" height="1028" alt="image" src="https://github.com/user-attachments/assets/6692a083-d6db-4a3b-a406-10c356eccd72" />
<img width="210" height="487" alt="image" src="https://github.com/user-attachments/assets/a7256abb-e221-42f4-bed1-831adebaad39" />

# 2. 프로젝트 clone 받아서 실행

### 1. 프로젝트 clone
```
cd ~
git clone https://github.com/kuks2309/ros2_3dslam_ws.git
cd ros2_3dslam_ws
ls src/
```

### 2. 패키지 설치
```
sudo apt update
sudo apt install ros-jazzy-gtsam
sudo apt install ros-jazzy-cv-bridge
sudo apt install libgtsam-dev
```

### 3. 빌드
```
cd ~/ros2_3dslam_ws
source /opt/ros/jazzy/setup.bash
colcon build
source install/setup.bash
```
<img width="813" height="135" alt="image" src="https://github.com/user-attachments/assets/2b5f9f61-1486-4313-a88f-5a92f8d0e231" />

### 4. 실행


#### 참고
.h 를 .hpp 로변경<br/>
#include <cv_bridge/cv_bridge.h> -> #include <cv_bridge/cv_bridge.hpp><br/>
#include "tf2_geometry_msgs/tf2_geometry_msgs.h" -> #include "tf2_geometry_msgs/tf2_geometry_msgs.hpp"<br/><br/>

ros2_3dslam_ws/src/SLAM/2D_SLAM/hector_slam_ros2/hector_trajectory_server/CMakeLists.txt 다음과 같이 변경<br/>
find_package(tf2_geometry_msgs) -> find_package(tf2_geometry_msgs REQUIRED)<br/><br/>

ros2_3dslam_ws/src/SLAM/2D_SLAM/hector_slam_ros2/hector_trajectory_server/package.xml 에 다음 depend 추가<br/>
 <build_depend>tf2_geometry_msgs</build_depend><br/>
 <run_depend>tf2_geometry_msgs</run_depend><br/><br/>

route_graph.hpp 에 다음 줄 추가<br/>
 #include <cstdint><br/>

 /home/iru/ros2_3dslam_ws/src/SLAM/3D_SLAM/LIO-SAM/livox_lio_sam/CMakeLists.txt 다음과 같이 변경<br/>
find_package(Eigen3 REQUIRED)<br/>
ament_target_dependencies(${PROJECT_NAME}_imuPreintegration rclcpp rclpy std_msgs sensor_msgs geometry_msgs nav_msgs pcl_conversions pcl_msgs visualization_msgs tf2 tf2_ros tf2_eigen tf2_sensor_msgs tf2_geometry_msgs OpenCV PCL GTSAM)<br/>
target_link_libraries(${PROJECT_NAME}_imuPreintegration gtsam Eigen3::Eigen "${cpp_typesupport_target}")<br/><br/>


 /home/iru/ros2_3dslam_ws/src/SLAM/3D_SLAM/LIO-SAM/livox_lio_sam/package.xml 다음과 같이 변경<br/>
 <build_depend>eigen</build_depend> -> <build_depend>eigen3_cmake_module</build_depend><br/>
 
```
ament_target_dependencies()에는 ROS/ament 패키지 이름만 들어감.
Eigen은 ROS 패키지명이 아니라 CMake/C++ 라이브러리(헤더 온리).
그래서 ament_target_dependencies(... Eigen)는 잘못된 사용이고, 오류가 남.
```

gazebo.launch.py 파일 변경 (하드코딩된 것 변경)<br/>
from ament_index_python.packages import get_package_share_directory
pkg_src = get_package_share_directory('tm_gazebo')
