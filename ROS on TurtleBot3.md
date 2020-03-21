# ROS on TurtleBot3
## 1.PC Setup
### 1.1 Install ROS 1 on Remote PC(need Terminal on Ubuntu 16.04)
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic.sh && chmod 755 ./install_ros_kinetic.sh && bash ./install_ros_kinetic.sh
```
### 1.2 Install Dependent ROS 1 Packages
```
$ sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server ros-kinetic-move-base ros-kinetic-urdf ros-kinetic-xacro ros-kinetic-compressed-image-transport ros-kinetic-rqt-image-view ros-kinetic-gmapping ros-kinetic-navigation ros-kinetic-interactive-markers
```
```
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
$ git clone https://github.com/ROBOTIS-GIT/turtlebot3.git
$ cd ~/catkin_ws && catkin_make
```

### 1.3 Network Configuration
To get the IP address of the Wi-Fi router(Remote PC connected), Use Terminal on Remote PC and Enter the command `$ ifconfig`.Then,
```
inet addr 192.168.x.xxx
```
Copy this IP address.
Next,Enter the command`$ nano ~/.bashrc`on another terminal.Then,
```
export ROS_MASTER_URI=http://192.168.y.yyy:11311
export ROS_HOSTNAME=192.168.y.yyy
```
Above the code,ROS_MASTER_URI and ROS_HOSTNAME values modify copied IP address.
```
export ROS_MASTER_URI=http://192.168.x.xxx:11311
export ROS_HOSTNAME=192.168.x.xxx
```
After modify,Enter the command `$ source ~/.bashrc`
