基于prometheus和T265实现室内无人机定点飞行
==========================================

1，消息流
---------

消息流如下图所示

.. image:: ../images/prometheus_px4_realsense/20201127005642921.png
 
2，软件和硬件环境 
-------------------

硬件

P200无人机

TX2 

Holybro pixhawk4 

T265 

软件 

1.10.0版本PX4固件

ubuntu16.04

nomachine（下载：\ https://www.nomachine.com/\ ，使用方法：\ `nomachine使用方法 <https://www.ncnynl.com/archives/202007/3809.html>`__\ ）

3，飞控参数修改 
-------------------

EKF2\_AID\_MASK 设置 视觉位置合成 和 视觉偏航合成

EKF2\_HGT\_MODE 设置为 Vision 使用视觉作为高度估计的主要来源。

.. image:: ../images/prometheus_px4_realsense/20201126224905726.png

.. image:: ../images/prometheus_px4_realsense/20201126224809796.png

4，软件环境搭建
---------------

4.1 安装T265驱动librealsense和ROS包
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

4.2 安装prometheus
^^^^^^^^^^^^^^^^^^

参考
`promethues安装与编译 <https://github.com/amov-lab/Prometheus/wiki/%E5%AE%89%E8%A3%85%E5%8F%8A%E7%BC%96%E8%AF%91>`__

5，硬件连接
-----------

准备下面这一条线 

.. image:: ../images/prometheus_px4_realsense/2020112711014747.png

\ TX2一端这么插

.. image:: ../images/prometheus_px4_realsense/20201127110050829.png

数据线的另一端接飞控telem2口，波特率为921600

6，软件修改
-----------

修改rs\_t265.launch里的设备号

rs\_t265.launch位于/home/nvidia/realsense\_ws/src/realsense-ros/realsense2\_camera/launch/文件夹下

如果运行roslaunch realsense2\_camera rs\_t265.launch出现以下报错

.. image:: ../images/prometheus_px4_realsense/20201203142716743.png

双击打开rs\_t265.launch文件，将下图红框内数字删除并保存此文件即可，数字两边的引号等等的不需要删除，仅需删除红框内数字。

.. image:: ../images/prometheus_px4_realsense/20201213194531547.png

7，命令运行 
-------------------

使用nomachine远程登录TX2

打开一个新终端，运行以下命令 roslaunch prometheus\_experiment
prometheus\_px4\_realsense.launch

8，飞行前的检查
---------------

rostopic echo /tf 消息打印正常代表T265节点起来运行正常 r

ostopic echo /mavros/vision\_pose/pose 消息打印正常代表px4\_pose\_estimate节点起来运行正常 

rostopic echo /mavros/state 为true代表MAVROS和飞控连接正常

打开地面站（此时建议用USB线连接电脑与飞控）看到LOCAL\_POSITION\_NED消息频率为30Hz，而且移动无人机看xyz数据的变化是否符合北东地坐标系，默认一开始机头方向为正东方向，具体表现为：向上移动无人机，Z的数据是往负方向变大，往左移动无人机是X的数据往正方向变大，往前方向也就是机头方向移动无人机，y往正方向变大。

.. image:: ../images/prometheus_px4_realsense/20201127000451770.png

9，实际飞行
-----------

以上检查确认正确之后 遥控器定点模式下解锁起飞即可。 演示视频如下。









