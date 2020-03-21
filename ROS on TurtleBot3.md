# ROS on TurtleBot3
## 1.Remote PC Setup(preinstalled Ubuntu 16.04)
### 1.1 Install ROS 1 on Remote PC
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
To get the IP address of the Wi-Fi router(Remote PC connected), Use Terminal on Remote PC and enter the command `$ ifconfig`.Then,
```
inet addr 192.168.x.xxx
```
Copy this IP address.
Next,enter the command`$ nano ~/.bashrc`on another terminal.Then,
```
export ROS_MASTER_URI=http://192.168.y.yyy:11311
export ROS_HOSTNAME=192.168.y.yyy
```
Above the code,`ROS_MASTER_URI` and `ROS_HOSTNAME` values modify copied IP address`192.168.x.xxx`.Then,
```
export ROS_MASTER_URI=http://192.168.x.xxx:11311
export ROS_HOSTNAME=192.168.x.xxx
```
After modified,save and exit nano(ctrl+x) and enter the command `$ source ~/.bashrc`

## 2.TurtleBot PC Setup(preinstalled "Raspbian for TurtleBot3" on Raspberry Pi 3)
Raspbian for TurtleBot3 download link(http://www.robotis.com/service/download.php?no=1738)
### 2.1 Install ROS 1 on "Raspbian for TurtleBot3"
After the installation, you can login with username: `pi` and password: `turtlebot`. Next,expand filesystem to use a whole SD card.
```
$ sudo raspi-config
(select 7 Advanced Options > A1 Expand Filesystem)
```
Next,with using NTP sync date and time.
```
$ sudo apt-get install ntpdate
$ sudo ntpdate ntp.ubuntu.com
```
### 2.2 Network Configuration
#### TurtleBot PC
```
nano ~/.bashrc
  (modify `localhost` to REMOTE_PC_IP and RASPBERRY_PI_3_IP)

  export ROS_MASTER_URI=http://REMOTE_PC_IP:11311 
  export ROS_HOSTNAME=RASPBERRY_PI_3_IP 
  source ~/.bashrc
```
REMOTE_PC_IP      :`192.168.x.xxx`(referenced §1.3)  
RASPBERRY_PI_3_IP : Enter the command`$ ifconfig` on TurtleBot PC(referenced §1.3)

therefore,
```
export ROS_MASTER_URI=http://192.168.x.xxx:11311 
export ROS_HOSTNAME=192.168.zzz.zzz 
```

#### Remote PC
Connect to TurtleBot PC via SSH from Remote PC.
```
ssh pi@192.168.zzz.zzz (The IP 192.168.zzz.zzz is your Raspberry Pi's IP or hostname)
```
