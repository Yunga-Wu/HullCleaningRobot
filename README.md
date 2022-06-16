## 机械结构
### 1.底盘：履带式
### 2.吸附机构：推进器吸附和永磁吸附相结合
- 推进器：[BlueRobot T200](https://item.taobao.com/item.htm?spm=a1z0k.7628869.0.0.1d3c37de90qw1V&id=550925052996&_u=t2dmg8j26111)，推进5Kgf
- 永磁铁：钕铁硼（N35）,尺寸100*50*20，需要30块
### 3.浮力材：密度0.46g/cm3  
![iamge](https://github.com/Yunga-Wu/HullCleaningRobot/blob/main/image/%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%95%B4%E4%BD%93%E8%AE%BE%E8%AE%A1%E5%B1%95%E7%A4%BA.png)

## 控制系统环境配置
1. 上位机：Raspberry Pi 4B, Ubuntu 20.04.4, ROS Noetic
2. 推进控制器：Pixhawk 飞控，ArduSub 固件
3. 底盘控制器：stm32
4. 双目相机： zed2
5. 地面站： windows 10, QGroundControl
6. 遥控手柄  
![iamge](https://github.com/Yunga-Wu/HullCleaningRobot/blob/main/image/%E6%8E%A5%E7%BA%BF%E5%9B%BE01.jpg)
### 配置记录
- ardusub官方镜像只支持raspberry 3 B，而最新的BlueOS支持raspberry系列较多型号，但是两者只有命令行界面
- 树莓派最终配置方案：Ubuntu 20.04.4，ROS Noetic，安装marvros功能包用于树莓派和pixhawk通信

## 参考资料
【1】[树莓派实验室 Homepage](https://shumeipai.nxez.com/tag/%E6%A0%91%E8%8E%93%E6%B4%BE): Learn how to use raspberry pi.
【2】[ArduPilot Homepage](https://ardupilot.org/ardupilot/index.html): ArduPilot enables the creation and use of trusted, autonomous, unmanned vehicle systems for the peaceful benefit of all.
【3】[Ardusub gitbook](http://www.ardusub.com/): The ArduSub project is a fully-featured open-source solution for remotely operated underwater vehicles (ROVs) and autonomous underwater vehicles (AUVs).
【4】[BlueOS](https://docs.bluerobotics.com/ardusub-zola/software/onboard/BlueOS-1.0/): the lateset modified version of Ardusub
【5】[QGroundControl](https://docs.qgroundcontrol.com/master/en/): QGroundControl provides full flight control and vehicle setup for PX4 or ArduPilot powered vehicles. 
【6】[Robotics Homepage](https://bluerobotics.com/learn/)
【7】[BlueROV ROS wiki](http://wiki.ros.org/Robots/BlueROV): The BlueROV is a remotely operated underwater vehicle from Blue Robotics. It features Blue Robotics thrusters, a flight controller with ArduSub software, and a RaspberryPi running ROS.
【8】[ROVMaker Forum](http://rovmaker.cn/): Underwater Robotics Chinese Forum
