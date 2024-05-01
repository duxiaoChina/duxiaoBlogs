# Ros2环境搭建

### 网上有好多教程，我这里记录一下自己安装过程

- 设置编码

    ```shell
    $ sudo apt update 
    $ sudo apt install locales
    $ sudo locale-gen en_US en_US.UTF-8
    $ sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
    $ export LANG=en_US.UTF-8

- 添加源

    ```shell
    通过检查此命令的输出，确保已启用Ubuntu Universe存储库。
    
    $ apt-cache policy | grep universe
    
    输出应如下：
    
    500 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages
         release v=22.04,o=Ubuntu,a=jammy-security,n=jammy,l=Ubuntu,c=universe,b=amd64
    
    若没有看到像上面这样的输出行，依次执行如下命令：
    
    $ sudo apt install software-properties-common
    $ sudo add-apt-repository universe
    
    继续执行如下命令：
    $ sudo apt update && sudo apt install curl gnupg lsb-release
    $ sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
    
    $ sudo curl -sSL https://raw.gitmirror.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
    
    $ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
    ```
    
    

    > 如果遇到报错“ailed to connect to raw.githubusercontent.com port 443 after 13 ms: 拒绝连接”
    > 可参考https://www.guyuehome.com/37844
    
    > 处理方法1:
    >
    > 直接把http://raw.githubusercontent.com换成raw.gitmirror.com(raw.githubusercontent.com的镜像网站)即可(后面的不用变)
    >
    > 处理方法2:
    >
    > ```shell
    > sudo vi /etc/hosts
    > #####################
    > 127.0.0.1	localhost
    > 127.0.1.1	iron-virtual-machine
    > ###增加下面的解析
    > 185.199.108.133  raw.githubusercontent.com
    > ```



- 安装ROS2

    ```shell
    $ sudo apt update
    $ sudo apt upgrade
    $ sudo apt install ros-humble-desktop
    ```

- 设置环境变量

    ```shell
    $ source /opt/ros/humble/setup.bash
    $ echo " source /opt/ros/humble/setup.bash" >> ~/.bashrc
    ```

- 测试

    ```shell
    $ ros2 run demo_nodes_cpp talker
    $ ros2 run demo_nodes_py listener
    
    $ ros2 run turtlesim turtlesim_node
    $ ros2 run turtlesim turtle_teleop_key
    ```

     ![Ros_Install_Test1.png]()

​	![Ros_Install_Test2.png]()
