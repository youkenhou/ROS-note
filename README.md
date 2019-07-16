# ROS-note
## 使ros的python功能在python3下也可以运行
结合了[这个博客](https://medium.com/@beta_b0t/how-to-setup-ros-with-python-3-44a69ca36674)和[这个问题](https://stackoverflow.com/questions/49221565/unable-to-use-cv-bridge-with-ros-kinetic-and-python3),成功实现了rospy在Python3下的使用.
首先应该已经安装好基于Python2的ROS，按照官网的wiki教程安装想要的版本即可。
然后利用`pip3`安装ROS的Python包：
```bash
sudo apt-get install python3-pip python3-yaml
sudo pip3 install rospkg catkin_pkg
```
经过这个操作之后`import rospy`之类的应该都没有问题了，但是`cv_bridge`仍然无法正常使用，需要利用Python3重新编译。
先查看一下自己的Python3版本`ls /usr/include/`，找一下类似`python3.5m`的文件夹。
然后按照以下步骤重新编译一个`cv_bridge`，先用Python3创建一个工作空间：
```bash
mkdir cv_bridge_python3
cd cv_bridge_python3
catkin init
catkin config -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.5m -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so
catkin config --install
```
其中`-DPYTHON_INCLUDE_DIR`和`-DPYTHON_LIBRARY`要按照实际情况修改。接下来在工作空间中：
```bash
mkdir src
cd src
git clone https://github.com/ros-perception/vision_opencv.git
cd ..
catkin build cv_bridge
```
如果没出错的话基于Python3的`cv_bridge`就编译好了。
要使当前的ROS程序使用这个包的话，在`cv_bridge_python3`下，`source install/setup.bash --extend`即可。
这个编译好的文件夹可以直接保存好，移动到其他工作空间。

其他可能还有些问题，但还没有遇到。
