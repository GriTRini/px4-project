# px4-project
## 1. px4 파일 설치
- 밑에 홈페이지에서 받으면 된다.  

홈페이지는 https://docs.px4.io/master/en/dev_setup/dev_env_linux_ubuntu.html#gazebo-jmavsim-and-nuttx-pixhawk-targets 이다.

- 파일 다운로드 

![Screenshot from 2021-07-19 09-05-19](https://user-images.githubusercontent.com/43773374/126086513-40fb3178-cbe2-4a4a-9a2b-e832fbe3b679.png)

```
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```
- 다운로드 받은후 입력
- Run the ubuntu.sh with no arguments (in a bash shell) to install everything:
![Screenshot from 2021-07-19 09-08-51](https://user-images.githubusercontent.com/43773374/126086516-e56d34a5-e21b-4e67-8830-aee0491e5ceb.png)

```
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

## 2. ros/gazebo 설치
- 해당 명령어를 입력해야 나중에 catkin build가 가능
- ros홈페이지에서 설치하는 방식대로 하면 catkin build가 되지 않음 
- ubuntu 18.04이며 melodic 설치이다.
- 밑에 내용은 해당 주소내용을 복사해오는 것이다.
![Screenshot from 2021-07-19 09-10-18](https://user-images.githubusercontent.com/43773374/126086529-9425df94-291f-42c3-991e-fb9eae0cdd78.png)

```
wget https://raw.githubusercontent.com/PX4/Devguide/master/build_scripts/ubuntu_sim_ros_melodic.sh
```
- 복사한 내용 실행
```
bash ubuntu_sim_ros_melodic.sh
```

- 만약에 오류가 뜨게 되면 밑의 명령어를 한번 입력한 후에 위의 명령어를 입력하면 된다. (경험상)

```
sudo rosdep init
rosdep update
```

## 3. gazebo 실행 해보기
```
cd PX4-Autopilot
```
- 위의 내용을 입력한 후에 밑의 명령어를 입력한후에 gazebo에서 드론이 나오는지 확인한다.
```
make px4_sitl gazebo
```

## 4. bash에 입력하기
- 명령어를 입력하면 PX4-Autopilot에 있는 내용을 roslaunch가 가능하다.
![Screenshot from 2021-07-19 09-11-17](https://user-images.githubusercontent.com/43773374/126086564-bc4041bd-74f5-49bd-9bb4-7c2ac64e28f8.png)

```
source ~/PX4-Autopilot/Tools/setup_gazebo.bash ~/PX4-Autopilot/ ~/PX4-Autopilot/build/px4_sitl_default

export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot

export ROS_PACKAGE_PATH=$ROS_PACKAGE_PATH:~/PX4-Autopilot/Tools/sitl_gazebo
```

```
source ~/.bashrc
```
- 위의 내용을 하고 나서 밑에 내용이 터미널 창에 뜨면 성공이다.
![Screenshot from 2021-07-19 09-11-59](https://user-images.githubusercontent.com/43773374/126086590-9f3a5a95-b8ea-4139-9b8d-3b6a2517a77d.png)

```
GAZEBO_PLUGIN_PATH :/home/jeong/PX4-Autopilot/build/px4_sitl_default/build_gazebo
GAZEBO_MODEL_PATH :/home/jeong/PX4-Autopilot//Tools/sitl_gazebo/models
LD_LIBRARY_PATH /home/jeong/catkin_ws/devel/lib:/opt/ros/melodic/lib:/home/jeong/PX4-Autopilot/build/px4_sitl_default/build_gazebo
```


## 5. github에서 사용할 custom_f450 프레임
- home 안에 설치한다.
```
git clone https://github.com/GriTRini/px4-project.git
```
- 안에 있는 px4-quadrotor-HW-parts-main 파일로 들어간다.
```
cd ~/px4-project/px4-quadrotor-HW-parts-main/
```
- px4-Autopilot model 안에 custom_f450을 넣는다.
```
cp -r custom_f450/ ~/PX4-Autopilot/Tools/sitl_gazebo/models/
```
- 1026_custom_f450 파일을 해당위치에 넣는다.
```
cp 1026_custom_f450 ~/PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes/
```
- px4_add_romfs_files 에 1026_custom_f450 을 적어서 추가한다.
![Screenshot from 2021-07-19 09-14-21](https://user-images.githubusercontent.com/43773374/126086667-44e8aae2-ae95-4138-86c2-380507d2f800.png)

```
gedit ~/PX4-Autopilot/ROMFS/px4fmu_common/init.d-posix/airframes/CMakeLists.txt
```
- set(models 안에 custom_f450을 적어서 추가한다.
![Screenshot from 2021-07-19 09-15-12](https://user-images.githubusercontent.com/43773374/126086682-c3685fc7-05c2-4a97-ab46-730273165d0d.png)

```
gedit ~/PX4-Autopilot/platforms/posix/cmake/sitl_target.cmake
```
- 위의 내용을 진행하였으면 roslaunch를 실행한다.
- iris 드론이 켜지는지 확인한다.
```
roslaunch px4 mavros_posix_sitl.launch
```
- 그다음 우리가 넣은 custom_f450드론을 실행시킨다.

- PX4-Autopilot / launch / mavros_posix_sitl.launch를 실행시킨다.
- 14행에 있는 iris를 custom_f450으로 수정한다.
- 만약 오류가 난다면 /home/jeong/.ros/etc/init.d-posix/airframes에서 1026_custom_f450파일을 추가한다.
- 그 다음 밑의 명령어를 실행시킨다.

```
roslaunch px4 mavros_posix_sitl.launch
```

- 만약 오류가 난다면 혹시 .ros 파일에 있는 airframe 안에 처음에 넣었던 1026_custom_f450 파일이 있는지 확인하라는 오류인지 확인해야한다.
- 저는 해당 오류가 떠서 .ros파일안 경로에 들어가서 처음에 복사 붙여넣기한 1026_custom_f450파일을 airframe 파일안에 넣었다.
- 그리고 나서 다시 실행하니 gazebo에서 실행이 되었다.

## 6. orb_slam2 설치
- rosdep 설치
```
sudo rosdep init
rosdep update
```
- Eigen3 설치
```
sudo apt install libeigen3-dev
```
- 먼저 위에 git에서 받은 파일중에 orb_slam_2_ros를 catkin_ws/src로 옮긴다.
```
cp -r px4-project/orb_slam_2_ros/ ~/catkin_ws/src
```
- launch 실행 파일 수정
```
cd catkin_ws/src/orb_slam_2_ros/ros/launch/ && gedit orb_slam2_d435_rgbd.launch
```
- line 5, 6 수정
![스크린샷, 2021-07-23 10-32-40](https://user-images.githubusercontent.com/43773374/126727994-3c25a017-12c8-48c2-924f-b8ec5dfe5e4a.png)
- line5 는 to에 /d435/rgb/image_raw
- line6 는 to에 /d435/depth/image_rect_raw

- build
```
cd ~/catkin_ws && catkin build
```
- 실행
```
roslaunch orb_slam2_ros orb_slam2_d435_rgbd.launch
```

## 7. qgroundcontrol을 설치한다.

- 해당 홈페이지에서 GCS를 설치한다.
![Screenshot from 2021-07-19 09-16-07](https://user-images.githubusercontent.com/43773374/126086709-6e422694-1f0d-4144-8ef1-8e224b7d0773.png)

https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html

- 
![Screenshot from 2021-07-19 09-18-26](https://user-images.githubusercontent.com/43773374/126086777-e09dbb74-f5f2-4b72-a25f-bb4c631d2580.png)

## 8. 다운받은 custom_f450 파일 rviz

```
cd px4-quadrotor-HW-parts-main/
```
```
cp -r f450 ~/catkin_ws/src
```
```
cd ~/catkin_ws && catkin build f450 && source devel/setup.bash
```
- 그다음 launch파일을 실행 시키면 된다.
```
roslaunch f450 display.launch
```
- 만약 오류가 난다면 오류 내용을 보고 패키지를 설치하면 된다.
- ![Screenshot from 2021-07-17 18-14-45](https://user-images.githubusercontent.com/43773374/126032754-35102308-6a0f-4731-baac-7660eb94e621.png)
- 저는 이러한 오류가 떴는데 해당 joint_state_publisher_gui를 설치한 후에 state_publisher를 설치하였다.
- melodic 버전을 확인하고 설치하면 된다.
- https://github.com/ros/joint_state_publisher 여기 사이트에 들어가서 주소를 복사한다음에 catkin build 실행
```
cd catkin_ws/src
```
```
git clone https://github.com/ros/joint_state_publisher.git
```
```
cd .. && catkin build
```
- 만약 state_publisher 오류가 뜬다면 밑의 내용을 실행한다.
```
cd catkin_ws/src
```
```
git clone https://github.com/ros/robot_state_publisher.git
```
```
cd .. && catkin build
```
![스크린샷, 2021-07-19 20-21-18](https://user-images.githubusercontent.com/43773374/126152811-1d583462-a290-4f5f-ad8c-43d224992538.png)

- 만약 사진과 같은 오류가 뜬다면 밑의 코드를 실행해라
```
sudo apt update
sudo apt install ros-<your_ros_version>-joint-state-publisher-gui
```
- ubuntu 16.04 이면 kinetic, ubuntu 18.04 이면 melodic, ubuntu 20.04 이면 noetic이다.
- map을 fix_frame으로 고정하고 드론을 띄우기 위해서는 send 의 value 값을 false 에서 true 로 바꿔주시면 map 에 base_link 가 정상적으로 연결됩니다.
```
gedit mavros/mavros/launch/px4_config.yaml
```
![스크린샷, 2021-07-23 10-40-53](https://user-images.githubusercontent.com/43773374/126729782-35e69aff-1653-4fa0-946f-4826185130bf.png)

- 이후에 밑에 내용을 실행한다.
```
 roslaunch f450 display.launch
 ```
 - 
 ![Screenshot from 2021-07-17 18-38-39](https://user-images.githubusercontent.com/43773374/126032892-b5fc2d74-c9f6-4fb7-9874-416852e96e14.png)
 

## 9. 실행

- 터미널 창에 명령어를 입력한다.
- gazebo 실행
```
roslaunch px4 mavros_posix_sitl.launch
```
- rviz 실행
```
roslaunch px4 display.launch
```
```
- slam2 실행
roslaunch orb_slam2_ros orb_slam2_d435_rgbd.launch
```

## 10. px4-avoidance 설치
- Install avoidance module dependencies (pointcloud library and octomap).
```
sudo apt install libpcl1 ros-melodic-octomap-*
```
- catkin/src 안에 avoidance 파일 다운로드
```
cd ~/catkin_ws/src
git clone https://github.com/PX4/avoidance.git
```
- catkin build
```
cd ~/catkin_ws && catkin build
```
- Qt-related errors 예방하기 위해서 .bashrc 에 넣기
```
export QT_X11_NO_MITSHM=1
```
- local_planner_stereo: simulates a vehicle with a stereo camera that uses OpenCV's block matching algorithm (SGBM by default) to generate depth information
```
roslaunch local_planner local_planner_stereo.launch
```
![스크린샷, 2021-07-26 16-41-42](https://user-images.githubusercontent.com/43773374/126951496-ee0ee9bd-b874-4190-8c4c-c807cf02c431.png)

- local_planner_depth_camera: simulates vehicle with one forward-facing kinect sensor
```
roslaunch local_planner local_planner_depth-camera.launch
```
![스크린샷, 2021-07-26 16-42-42](https://user-images.githubusercontent.com/43773374/126951634-954926f2-fdac-4118-98e6-8c10b4626664.png)

- local_planner_sitl_3cam: simulates vehicle with 3 kinect sensors (left, right, front)
```
roslaunch local_planner local_planner_sitl_3cam.launch
```
![스크린샷, 2021-07-26 16-43-33](https://user-images.githubusercontent.com/43773374/126951792-e2aa4f49-c6fb-4e78-84f0-7be1f9aac8dc.png)

- Global Planner (advanced, not flight tested) This section shows how to start the global_planner and use it for avoidance in offboard mode.
```
roslaunch global_planner global_planner_stereo.launch
```
![스크린샷, 2021-07-26 16-44-22](https://user-images.githubusercontent.com/43773374/126951881-585879f5-6ec2-4861-a83e-4b7a101d312d.png)

- 해당 코드를 실행하게 되면 왼쪽 오른쪽 카메라가 보여지고 전면 카메라가 보이게 된다.
```
rosrun image_view stereo_view stereo:=/stereo image:=image_rect_color
```
![스크린샷, 2021-07-26 16-45-13](https://user-images.githubusercontent.com/43773374/126952151-1336b3bc-328e-40a3-bb67-43cb96f5f4b4.png)

- 수정없이 그냥 하게 된다면 gazebo가 실행되지 않고 rviz만 실행이 된다. gazebo도 실행을 시키려면 밑의 사진대로 하면 된다.
```
cd ~/catkin_ws/src/avoidance/avoidance/launch && gedit avoidance_sitl_mavros.launch
```
![스크린샷, 2021-07-26 16-40-05](https://user-images.githubusercontent.com/43773374/126951252-5a3446d5-c36e-4203-85d9-086fc38e92d4.png)
- line 3에 있는 false를 true로 바꾸게 되면 gazebo
