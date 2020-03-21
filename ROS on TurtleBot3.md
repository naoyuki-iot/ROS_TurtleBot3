# ROS on TurtleBot3 Burger
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
Next,enter the command`$ nano ~/.bashrc`on another terminal.  
Therefore,
```
export ROS_MASTER_URI=http://192.168.y.yyy:11311
export ROS_HOSTNAME=192.168.y.yyy
```
Above the script,`ROS_MASTER_URI` and `ROS_HOSTNAME` values modify copied IP address`192.168.x.xxx`.Then,
```
export ROS_MASTER_URI=http://192.168.x.xxx:11311
export ROS_HOSTNAME=192.168.x.xxx
```
After modified the value,save and exit nano(ctrl+x) and enter the command `$ source ~/.bashrc`

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
##### bash
```
$ nano ~/.bashrc
  (modify `localhost` to REMOTE_PC_IP and RASPBERRY_PI_3_IP)
```
##### nano
```
  export ROS_MASTER_URI=http://REMOTE_PC_IP:11311 
  export ROS_HOSTNAME=RASPBERRY_PI_3_IP 
```
REMOTE_PC_IP      :`192.168.x.xxx`(referenced §1.3)  
RASPBERRY_PI_3_IP : Enter the command`$ ifconfig` on TurtleBot PC(referenced §1.3)  

Therefore,
```
export ROS_MASTER_URI=http://192.168.x.xxx:11311 
export ROS_HOSTNAME=192.168.zzz.zzz 
```
save and exit nano.
##### bash
```
$ source ~/.bashrc
```
#### Remote PC
Connect to TurtleBot PC via SSH from Remote PC.
```
$ ssh pi@192.168.zzz.zzz (The IP 192.168.zzz.zzz is your Raspberry Pi's IP or hostname)
```
## 3.OpenCR Setup
### 3.1 OpenCR Firmware Upload for TurtleBot3 Burger
Run the shell script below.
```
$ export OPENCR_PORT=/dev/ttyACM0
$ export OPENCR_MODEL=burger
$ rm -rf ./opencr_update.tar.bz2
$ wget https://github.com/ROBOTIS-GIT/OpenCR-Binaries/raw/master/turtlebot3/ROS1/latest/opencr_update.tar.bz2 && tar -xvf opencr_update.tar.bz2 && cd ./opencr_update && ./update.sh $OPENCR_PORT $OPENCR_MODEL.opencr && cd ..
```
## 4.Bringup ROS
### 4.1 Run roscore
#### Remote PC
Enter the command `$ roscore`
### 4.2 Run roslaunch
#### TurtleBot PC
Enter the command below.
```$ roslaunch turtlebot3_bringup turtlebot3_robot.launch```
Then,the terminal will represent below messages.
```
SUMMARY
========

PARAMETERS
 * /rosdistro: kinetic
 * /rosversion: 1.12.13
 * /turtlebot3_core/baud: 115200
 * /turtlebot3_core/port: /dev/ttyACM0
 * /turtlebot3_core/tf_prefix: 
 * /turtlebot3_lds/frame_id: base_scan
 * /turtlebot3_lds/port: /dev/ttyUSB0

NODES
  /
    turtlebot3_core (rosserial_python/serial_node.py)
    turtlebot3_diagnostics (turtlebot3_bringup/turtlebot3_diagnostics)
    turtlebot3_lds (hls_lfcd_lds_driver/hlds_laser_publisher)

ROS_MASTER_URI=http://192.168.x.xxx:11311

process[turtlebot3_core-1]: started with pid [14198]
process[turtlebot3_lds-2]: started with pid [14199]
process[turtlebot3_diagnostics-3]: started with pid [14200]
[INFO] [1531306690.947198]: ROS Serial Python Node
[INFO] [1531306691.000143]: Connecting to /dev/ttyACM0 at 115200 baud
[INFO] [1531306693.522019]: Note: publish buffer size is 1024 bytes
[INFO] [1531306693.525615]: Setup publisher on sensor_state [turtlebot3_msgs/SensorState]
[INFO] [1531306693.544159]: Setup publisher on version_info [turtlebot3_msgs/VersionInfo]
[INFO] [1531306693.620722]: Setup publisher on imu [sensor_msgs/Imu]
[INFO] [1531306693.642319]: Setup publisher on cmd_vel_rc100 [geometry_msgs/Twist]
[INFO] [1531306693.687786]: Setup publisher on odom [nav_msgs/Odometry]
[INFO] [1531306693.706260]: Setup publisher on joint_states [sensor_msgs/JointState]
[INFO] [1531306693.722754]: Setup publisher on battery_state [sensor_msgs/BatteryState]
[INFO] [1531306693.759059]: Setup publisher on magnetic_field [sensor_msgs/MagneticField]
[INFO] [1531306695.979057]: Setup publisher on /tf [tf/tfMessage]
[INFO] [1531306696.007135]: Note: subscribe buffer size is 1024 bytes
[INFO] [1531306696.009083]: Setup subscriber on cmd_vel [geometry_msgs/Twist]
[INFO] [1531306696.040047]: Setup subscriber on sound [turtlebot3_msgs/Sound]
[INFO] [1531306696.069571]: Setup subscriber on motor_power [std_msgs/Bool]
[INFO] [1531306696.096364]: Setup subscriber on reset [std_msgs/Empty]
[INFO] [1531306696.390979]: Setup TF on Odometry [odom]
[INFO] [1531306696.394314]: Setup TF on IMU [imu_link]
[INFO] [1531306696.397498]: Setup TF on MagneticField [mag_link]
[INFO] [1531306696.400537]: Setup TF on JointState [base_link]
[INFO] [1531306696.407813]: --------------------------
[INFO] [1531306696.411412]: Connected to OpenCR board!
[INFO] [1531306696.415140]: This core(v1.2.1) is compatible with TB3 Burger
[INFO] [1531306696.418398]: --------------------------
[INFO] [1531306696.421749]: Start Calibration of Gyro
[INFO] [1531306698.953226]: Calibration End
```
### 4.3 Load a TurtleBot3 on Rviz
Rviz is a simulation software for ROS compatible robots.(Will be used in the next chapter.)
#### Remote PC
Launch robot state publisher.
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_bringup turtlebot3_remote.launch
```
And run Rviz.
```
$ rosrun rviz rviz -d `rospack find turtlebot3_description`/rviz/model.rviz
```
## 5.Basic Operation
### 5.1 Teleoperation - Keyboard control from Remote PC
Launch `turtlebot3_teleop_key`.
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```
Then,
```
 Control Your Turtlebot3!
  ---------------------------
  Moving around:
          w
     a    s    d
          x

  w/x : increase/decrease linear velocity
  a/d : increase/decrease angular velocity
  space key, s : force stop

  CTRL-C to quit
```
(Tips)It can be controlled not only with the keyboard but also with the game controller(Wii,PS3,Xbox 360). -> comming soon.

## 6.SLAM
SLAM - Simultaneous Localization and Mapping
### 6.1 Run SLAM
#### Remote PC
Run `$roscore`.
#### TurtleBot PC
Bring up basic packages to start TurtleBot3 applications.

```$ roslaunch turtlebot3_bringup turtlebot3_robot.launch```
#### Remote PC
Open a new terminal and launch the SLAM file.
```
$ export TURTLEBOT3_MODEL=$burger
$ roslaunch turtlebot3_slam turtlebot3_slam.launch slam_methods:=gmapping
```
### 6.2 Run Teleoperation(referenced §5.1)
```
$ export TURTLEBOT3_MODEL=burger
$ roslaunch turtlebot3_teleop turtlebot3_teleop_key.launch
```
Then,
```
 Control Your TurtleBot3!
  ---------------------------
  Moving around:
          w
     a    s    d
          x

  w/x : increase/decrease linear velocity
  a/d : increase/decrease angular velocity
  space key, s : force stop

  CTRL-C to quit
```
### 6.3 Save Map(after exploration)
#### Remote PC
Enter the command `$ rosrun map_server map_saver -f ~/map`
