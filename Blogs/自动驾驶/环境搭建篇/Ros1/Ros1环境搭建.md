# Ros1环境搭建

---

## 一、前期配置

- 安装VsCode
- 安装NoMachine远程桌面
- 安装proxychains 代理

##  二、Ubuntu1804安装 Ros1 Melodic

```shell
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
sudo proxychains apt-get update
sudo proxychains apt install ros-melodic-desktop-full
sudo proxychains apt install rospack-tools
sudo proxychains rosdep init
proxychains rosdep update
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc && source ~/.bashrc
sudo proxychains apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```

## 三、必备库安装

```shell
sudo proxychains apt-get install ros-melodic-qt-create &&
sudo proxychains apt-get install ros-melodic-qt-build &&
sudo proxychains apt-get install ros-melodic-qt-ros &&
sudo proxychains apt-get install qtmultimedia5-dev &&
sudo proxychains apt-get install libqt5serialport5 &&
sudo proxychains apt-get install libqt5serialport5-dev &&
sudo proxychains apt-get install libqt5webenginewidgets5 &&
sudo proxychains apt install qtwebengine5-dev &&
sudo proxychains apt-get install ros-melodic-serial &&
sudo proxychains apt-get install ros-melodic-ddynamic-reconfigure &&
sudo proxychains apt-get install ros-melodic-imu-tools &&
sudo proxychains apt-get install pcl-tools &&
sudo proxychains apt-get install ros-melodic-geographic-msgs &&
sudo proxychains apt-get install ros-melodic-perception &&
sudo apt-get install ros-melodic-imu-tools ros-melodic-rviz-imu-plugin

sudo proxychains apt install python-pip

pip install setproctitle
pip install paho-mqtt
pip install apscheduler
pip install python-can

Deng开发环境
sudo proxychains apt-get install libgoogle-glog-dev &&
sudo proxychains apt-get install ros-melodic-jsk-recognition-msgs &&
sudo proxychains apt-get install ros-melodic-jsk-rviz-plugins &&
sudo proxychains apt-get install libpcap-dev &&
sudo proxychains apt-get install ros-melodic-serial

图像处理相关
sudo apt-get install python-scipy
sudo apt-get install python-numpy
sudo apt-get install python-pyproj

pip install -i https://pypi.tuna.tsinghua.edu.cn/simple opencv-python==4.2.0.32


#会安装Python3(别装)
sudo apt-get install ros-melodic-catkin-virtualenv

sudo apt-get install ros-melodic-jsk-recognition-msgs & sudo apt-get install ros-melodic-jsk-rviz-plugins

```

## 四、NDT Lib安装(Ubuntu 1604下使用GPU[CUDA9]无问题)

> 改库使用显卡GPU建图效率较高，但以来英伟达显卡及显卡驱动和CUDA 9.
>
> 目前只在Ubunt1604安装英伟达1660显卡及CUDA9.0成功。
>
> 在其他版本如Ubuntu1804上编译GPU的库失败。

```shell
mkdir -p ~/ndt-lib/src
cd ~/ndt-lib/src
git clone https://github.com/zju-sclab/NDT-library.git
cd ..
catkin_make -DCMAKE_BUILD_TYPE=Release
cd devel/lib
sudo mkdir /usr/lib/ndt_lib
sudo cp -r * /usr/lib/ndt_lib/
```

## 五、其他附加库

```shell
# 欧几里建图
sudo apt-get install ros-kinetic-jsk-recognition-msgs 
sudo apt-get install ros-kinetic-jsk-rviz-plugins

sudo apt-get install ros-melodic-plotjuggler
rosrun plotjuggler plotjuggler  
```
