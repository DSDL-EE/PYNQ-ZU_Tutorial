# ROS Installation on PYNQ-ZU

## Contents
- [System Check](#system-check)
- [1. ROS Noetic Installation](#1-ros-noetic-installation)
  - [1.1. Setup your sources.list](#11-setup-your-sourceslist)
  - [1.2. Set up your keys](#12-set-up-your-keys)
  - [1.3. Installation](#13-installation)
  - [1.4. Environment setup](#14-environment-setup)
  - [1.5. Dependencies for building packages](#15-dependencies-for-building-packages)
    - [1.5.1. Initialize rosdep](#151-initialize-rosdep)
  - [1.6. ROS Core Smoke Test](#16-ros-core-smoke-test)
  - [1.7. Install extra packages \& applications](#17-install-extra-packages--applications)
- [2. Example with Kobuki Robot](#2-example-with-kobuki-robot)
  - [2.1. Prepare the workspace to compile ROS components \& drivers for Kobuki](#21-prepare-the-workspace-to-compile-ros-components--drivers-for-kobuki)
  - [2.2. Compile Kobuki source code](#22-compile-kobuki-source-code)
  - [2.3. Install pre-built dependencies for Kobuki workspace](#23-install-pre-built-dependencies-for-kobuki-workspace)
  - [2.4. Compile ROS components for Kobuki](#24-compile-ros-components-for-kobuki)
  - [2.5. Configure Kobuki to integrate with system](#25-configure-kobuki-to-integrate-with-system)
  - [2.6. Install Kobuki's chassis driver](#26-install-kobukis-chassis-driver)
  - [2.7. Bring-up Test](#27-bring-up-test)
- [3. PC Setup (Optional)](#3-pc-setup-optional)
  - [3.1. Download and Install Ubuntu on PC](#31-download-and-install-ubuntu-on-pc)
  - [3.2. Install ROS on Remote PC](#32-install-ros-on-remote-pc)
  - [3.3. Install Dependent ROS Packages](#33-install-dependent-ros-packages)
  - [3.4. Configuration](#34-configuration)
  - [3.5. Launch](#35-launch)
- [Reference](#reference)

## System Check
Review the PYNQ image's information by running:
```
lsb_release -a
```
It should show:
```
No LSB modules are available.
Distributor ID:	Pynqlinux
Description:	PynqLinux, based on Ubuntu 20.04
Release:	2.7
Codename:	Austin
```

Install some basic tools.
```
sudo apt purge --remove vim-tiny
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install vim net-tools
```

## 1. ROS Noetic Installation
### 1.1. Setup your sources.list
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu focal main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt update
```

### 1.2. Set up your keys
```
sudo apt install curl # if you haven't already installed curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo apt update
```

### 1.3. Installation
Although there are several options, ROS Desktop-Full is recommended.
```
sudo apt install ros-noetic-desktop-full
```

### 1.4. Environment setup
You must source this script in every bash terminal you use ROS in.
```
source /opt/ros/noetic/setup.bash
```

Because PYNQ is based on Ubuntu, and assigned a different codename which does not match any supported OS by default. We need to set ROS_OS_OVERRIDE variable. For PYNQ image version 2.7, we set:
```
export ROS_OS_OVERRIDE=ubuntu:focal
```

It can be convenient to automatically source this script every time a new shell is launched. These commands will do that for you.
```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
echo "export ROS_OS_OVERRIDE=ubuntu:focal" >> ~/.bashrc
source ~/.bashrc
```

### 1.5. Dependencies for building packages
Up to now you have installed what you need to run the core ROS packages. To create and manage your own ROS workspaces, there are various tools and requirements that are distributed separately. For example, rosinstall is a frequently used command-line tool that enables you to easily download many source trees for ROS packages with one command.

To install this tool and other dependencies for building ROS packages, run:
```
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
```

#### 1.5.1. Initialize rosdep
Before you can use many ROS tools, you will need to initialize rosdep. rosdep enables you to easily install system dependencies for source you want to compile and is required to run some core components in ROS. If you have not yet installed rosdep, do so as follows.
```
sudo apt install python3-rosdep
```
With the following, you can initialize rosdep.
```
sudo rosdep init
rosdep update
sudo reboot now
```

### 1.6. ROS Core Smoke Test
After the system is rebooted, a smoke test can be excuted to validate ROS installation.
```
roscore
```

The expected result shoud be:
```
SUMMARY
========

PARAMETERS
 * /rosdistro: noetic
 * /rosversion: 1.16.0

NODES

auto-starting new master
process[master]: started with pid [1534]
ROS_MASTER_URI=http://pynq:11311/

setting /run_id to ed1894dc-ce52-11ed-80bd-4549babc14d7
process[rosout-1]: started with pid [1544]
started core service [/rosout]
```

### 1.7. Install extra packages & applications
There are even more packages available in ROS. You can always install a specific package directly.
```
sudo apt install ros-noetic-PACKAGE
```
e.g.
```
sudo apt install ros-noetic-slam-gmapping
```

To find available packages, see ROS Index or use:
```
apt search ros-noetic
```

## 2. Example with Kobuki Robot
This example shows how to integrate PYNQ-ZU as a controller, with a ROS-based robot like [Yujin iClebo Kobuki](https://github.com/yujinrobot/kobuki).

### 2.1. Prepare the workspace to compile ROS components & drivers for Kobuki
On PYNQ-ZU, run:
```
mkdir kobuki_ws && cd kobuki_ws
mkdir src
catkin_make
rosdep install --from-paths src --ignore-src -r -y
```

### 2.2. Compile Kobuki source code
```
cd ~/kobuki_ws/src
git clone https://github.com/yujinrobot/kobuki.git
git clone https://github.com/yujinrobot/yujin_ocs.git
```

Remove unnecessary packages:
```
cd ~/kobuki_ws/src/yujin_ocs
rm -r yocs_ar_marker_tracking yocs_ar_pair_approach \
 yocs_ar_pair_tracking yocs_diff_drive_pose_controller \
 yocs_joyop yocs_keyop yocs_localization_manager \
 yocs_math_toolkit yocs_navi_toolkit yocs_navigator \
 yocs_rapps yocs_safety_controller yocs_virtual_sensor \
 yocs_waypoint_provider yocs_waypoints_navi yujin_ocs
```

### 2.3. Install pre-built dependencies for Kobuki workspace
On PYNQ-ZU, run:
```
cd ~/kobuki_ws
sudo apt-get install -y ros-noetic-kobuki-*
sudo apt-get install -y liborocos-kdl-dev
sudo apt-get install -y ros-noetic-ecl-*
```

### 2.4. Compile ROS components for Kobuki
```
cd ~/kobuki_ws
catkin_make
```

### 2.5. Configure Kobuki to integrate with system
```
echo "source $HOME/kobuki_ws/devel/setup.bash" >> ~/.bashrc
```

### 2.6. Install Kobuki's chassis driver
```
cd ~/kobuki_ws/src
git clone https://github.com/turtlebot/turtlebot.git
git clone https://github.com/turtlebot/turtlebot_msgs.git
git clone https://github.com/ros-drivers/linux_peripheral_interfaces.git
cd linux_peripheral_interfaces
rm -r linux_peripheral_interfaces libsensors_monitor
cd ~/kobuki_ws/src/turtlebot
rm -r turtlebot_teleop/
sudo apt-get install -y ros-noetic-move-base*
sudo apt-get install -y ros-noetic-driver-base
sudo apt-get install -y python
cd ~/kobuki_ws
catkin_make
sudo reboot now
```

### 2.7. Bring-up Test
At first, connect PYNQ-ZU to WiFi then use `ifconfig wlan0` to retrieve its IP address.

Then, on your PC, open Terminal or MobaXterm and run:
```
ssh xilinx@PYNQ-ZU_IP
roscore &
roslaunch turtlebot_bringup minimal.launch &
roslaunch kobuki_keyop keyop.launch
```

Now you can use arrow buttons to adjust velocity and rotation of Kobuki robot. If everything is out-of-control, remember to press 'Space' immediately to reset velocity.

### 2.8. Extensions
After the ROS Core is installed successfully, user can attach peripherals to the PYNQ-ZU & Kobuki for more development.

## 3. PC Setup (Optional)
This section is optional if you want to host ROS Core on PC instead of PYNQ-ZU board. In this scenario, PYNQ-ZU will be a ROS client connect to the PC.

### 3.1. Download and Install Ubuntu on PC
It is suggested to install VirtualBox - a virtual machines application, then install Ubuntu Desktop 20.04 on it.
- [Download VirtualBox](https://www.virtualbox.org/wiki/Downloads)
- [Download Ubuntu Desktop 20.04 LTS](https://releases.ubuntu.com/20.04/)

### 3.2. Install ROS on Remote PC
After the installation, we will install some basic tools.
```
sudo apt update
sudo apt install vim net-tools
```

Now we can install the ROS.
```
sudo apt upgrade
wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_noetic.sh
chmod 755 ./install_ros_noetic.sh 
bash ./install_ros_noetic.sh
```

### 3.3. Install Dependent ROS Packages
```
sudo apt-get install -y ros-noetic-joy ros-noetic-teleop-twist-joy \
  ros-noetic-teleop-twist-keyboard ros-noetic-laser-proc \
  ros-noetic-rgbd-launch ros-noetic-rosserial-arduino \
  ros-noetic-rosserial-python ros-noetic-rosserial-client \
  ros-noetic-rosserial-msgs ros-noetic-amcl ros-noetic-map-server \
  ros-noetic-move-base ros-noetic-urdf ros-noetic-xacro \
  ros-noetic-compressed-image-transport ros-noetic-rqt* ros-noetic-rviz \
  ros-noetic-gmapping ros-noetic-navigation ros-noetic-interactive-markers
```

### 3.4. Configuration
To operate the host-client mechanism, we need to set the ROS_MASTER_URI and ROS_HOSTNAME variables.

On PC, configure:
```
export PC_IP=192.168.1.103 # Change this line to match your PC's IP
echo 'export ROS_MASTER_URI=http://'$PC_IP':11311' >> ~/.bashrc
echo 'export ROS_HOSTNAME='$PC_IP >> ~/.bashrc
source ~/.bashrc
```

On PYNQ-ZU, configure:
```
export BOARD_IP=192.168.1.103 # Change this line to match your PC's IP
echo 'export ROS_MASTER_URI=http://'$BOARD_IP':11311' >> ~/.bashrc
echo 'export ROS_HOSTNAME='$BOARD_IP >> ~/.bashrc
source ~/.bashrc
```

### 3.5. Launch
On PC, run:
```
roscore
```

On PYNQ-ZU, run:
```
roslaunch <program>
```

## Reference
[1] http://wiki.ros.org/noetic/Installation/Ubuntu

[2] http://wiki.ros.org/ROS/EnvironmentVariables

[3] https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/

[4] https://github.com/yujinrobot/kobuki
