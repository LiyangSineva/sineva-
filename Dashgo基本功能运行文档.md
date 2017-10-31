# Dashgo基本功能运行文档
`李阳`
## 简介
本文将简单说明运行Dashgo的方法.
### 环境需求
下图为Dashgo硬件:
![Dashgo硬件](https://striker.teambition.net/storage/110y5c6faabb856b971014134418000b386e?download=2017-10-31%2012-28-23%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png&Signature=eyJhbGciOiJIUzI1NiJ9.eyJyZXNvdXJjZSI6Ii9zdG9yYWdlLzExMHk1YzZmYWFiYjg1NmI5NzEwMTQxMzQ0MTgwMDBiMzg2ZSIsImV4cCI6MTUwOTU4MDgwMH0.zbOFHYCD2HJV6JMkmsWFIShGpNvTofKh-T7x2BaYhrk)
要用到的硬件分为独立的4部分,分别是:
1.  Dashgo机器人,由以下组件组成:
  1.  北阳激光雷达
  1.  upboard控制板
  1.  wifi网卡
  1.  USB数据线
1.  Dashgo上的小米遥控手柄
1.  黄色充电器
1.  台式电脑(没有体现在图中),带有蓝牙接收器用于链接手柄.

### 背景知识
Dashgo上的Upboard是一台X86控制器,装有Ubuntu,在本设计中,我们在台式电脑上借助SSH与它进行通信.使用NFS挂载修改其中的文件.</br>
运行此程序至少需要了解ssh的链接方法,本文中不做描述.</br>
运行者需要熟悉Linux命令行,本文中不做描述.</br>

### 变量设定
这里设定,Dashgo和台式机都链接到了路由器```192.168.31.1```</br>
Dashgo的IP地址为```192.168.31.81```</br>
台式机的IP地址为```192.168.31.80```</br>
这些信息将在下文中用到,如果测试环境与之不同,需要注意进行修改.

## 运行流程
1.  使用nfs链接Dashgo,修改其中的.bashrc 确保其中包含ROS的导入命令和ROS分布式地址:
```sh
source /opt/ros/kinetic/setup.bash
source /home/lee8871/ros-dashgo/devel/setup.bash
export ROS_MASTER_URI=http://192.168.31.81:11311
export ROS_HOSTNAME=192.168.31.81
```
    使用鼠标和显示器链接也可以达到相同的效果.
1.  修改台式机器的.bashrc 确保其中包含ROS的导入命令和ROS分布式地址:
```sh
source /opt/ros/kinetic/setup.bash
source /home/lee8871/ros-dashgo/devel/setup.bash
export ROS_MASTER_URI=http://192.168.31.81:11311
export ROS_HOSTNAME=192.168.31.80
```
1.  打开终端链接SSH到Dashgo,运行命令
```sh
sudo chmod 777 /dev/ttyUSB0
roscore
```
![](https://striker.teambition.net/storage/110y59404c12e341fbc090908aed55d17065?download=2017-10-30%2016-59-02%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png&Signature=eyJhbGciOiJIUzI1NiJ9.eyJyZXNvdXJjZSI6Ii9zdG9yYWdlLzExMHk1OTQwNGMxMmUzNDFmYmMwOTA5MDhhZWQ1NWQxNzA2NSIsImV4cCI6MTUwOTQ5NDQwMH0._pW4amdPZh7olL9RCnC37VallxP03Jdvt5D8Q_lWXiY)
1.  打开终端链接到Dashgo,运行命令
```sh
roslaunch dashgo_bringup dashgo_lidar_sonar_odomerty.launch
```
1.  启动小米手柄，链接到台式机。此时可能会发现，小米手柄会控制台式机的鼠标则运行这条命令
```sh
xinput set-prop "小米蓝牙手柄" "Device Enabled" 0
```
1.  打开终端运行命令:
```sh
roslaunch dashgo_desktop gxzn-and-rviz.launch
```
看到以下图像,并且手柄可以控制机器人，说明运行成功。</br>
手柄如何控制参见下文！！！！！</br>
手柄如何控制参见下文！！！！！</br>
手柄如何控制参见下文！！！！！</br>
![](https://striker.teambition.net/storage/110ya1635db63c399959f4585fe671aee283?download=2017-10-30%2017-08-10%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png&Signature=eyJhbGciOiJIUzI1NiJ9.eyJyZXNvdXJjZSI6Ii9zdG9yYWdlLzExMHlhMTYzNWRiNjNjMzk5OTU5ZjQ1ODVmZTY3MWFlZTI4MyIsImV4cCI6MTUwOTQ5NDQwMH0.2GTlXRwHAnbRUgbx3U8lOFkMQOkRwGghqia6irCaDjM)

## 补充说明
### 关于RVIZ图像
1.  图像中的黄点是声呐采集到的信息，蓝色点是激光采集到的信息
1.  多个坐标为车体当前位置，紫色的箭头为里程计给出的位置
1.  精细的粗糙的地图分别是激光和声呐单独创造的地图，声呐精度过低，无法进行SLAM
1.  你可以控制机器人在房屋内巡视，这样可以在RVIZ中看到完整的地图；

### 关于控制
按下L1后，手柄控制才能使用，手柄的左侧二维摇杆控制Dashgo的速度与方向，机器人的旋转速度和移动速度随着推动摇杆的深度提高，最高速度是0.2m每秒，如果同时按下R2，Dashgo的最高速度将根据R2按下的深度提高。</br>
按下L2，Dashgo将以急速运行，不推荐这样的操作。</br>

### 关于已知的BUG
1.  手柄控制的最高速度在刚刚启动时会有异常，解决方法是每次启动后先按下R2一次。
1.  机器人雷达显示和墙面总是不匹配，需要更精确的测量雷达的位置才能校准此误差，目前不准备处理。
1.  雷达总是出现异常数据，无法处理，需要更好的硬件。

</br></br></br></br></br></br></br></br></br></br></br></br></br></br>
