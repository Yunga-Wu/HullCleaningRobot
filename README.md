# 软硬件框架搭建
## 机械结构
1. 底盘：履带式
2. 吸附机构：推进器吸附和永磁吸附相结合
   - 推进器：[BlueRobot T200](https://item.taobao.com/item.htm?spm=a1z0k.7628869.0.0.1d3c37de90qw1V&id=550925052996&_u=t2dmg8j26111)，推进5Kgf
   - 永磁铁：钕铁硼（N35）,尺寸100*50*20，需要30块
3. 浮力材：密度0.46g/cm3 

![iamge](https://github.com/Yunga-Wu/HullCleaningRobot/blob/main/image/%E6%9C%BA%E5%99%A8%E4%BA%BA%E6%95%B4%E4%BD%93%E8%AE%BE%E8%AE%A1%E5%B1%95%E7%A4%BA.png)

## 控制系统环境配置
1. 上位机：Raspberry Pi 4B, Ubuntu 20.04.4, ROS Noetic
2. 推进控制器：Pixhawk 2.4.8 飞控，ArduSub 固件
3. 底盘控制器：stm32
4. 双目相机： zed2
5. 地面站： windows 10, QGroundControl
6. 遥控手柄  

![iamge](https://github.com/Yunga-Wu/HullCleaningRobot/blob/main/image/%E6%8E%A5%E7%BA%BF%E5%9B%BE01.jpg)

### 配置记录
#### raspberry pi
- 树莓派最终配置方案：Ubuntu 20.04.4，ROS Noetic，安装marvros功能包用于树莓派和pixhawk通信
- 将该系统备份，生成镜像，以后直接使用配置好的镜像
- 如果想用官方[Ardusub-Raspbian镜像](http://www.ardusub.com/resources/downloads.html#ardusub-firmware-files)，需要使用raspberry 3 B版本，其他版本的树莓派可以使用[BlueOS镜像](https://docs.bluerobotics.com/ardusub-zola/software/onboard/BlueOS-1.0/installation/)(账号：pi，密码：raspberry)，但是两者只有命令行界面
#### pixhawk
- 在Ardusub gitbook中下载[v4.0.2](http://www.ardusub.com/resources/downloads.html)版本firmware，得到一个.apj格式的固件镜像
- 用USB将pixhawk与地面站相连，地面站会识别到飞控，在QGroundControl的firmware界面中选择手动选择固件，选中上一步下载的ardusub固件镜像，点击确定，自动烧写到飞控中

![image](https://github.com/Yunga-Wu/HullCleaningRobot/blob/main/image/%E9%A3%9E%E6%8E%A7%E5%9B%BA%E4%BB%B6%E7%83%A7%E5%86%99.png)

- 后期二次开发时，将ardupilot编译生成的.apj文件（一般在bin文件夹）按照这种方式就可以烧写到飞控中，进而更新飞控固件

#### Windows配置
- 主要是IP地址设置，参考[Network Setup](http://www.ardusub.com/quick-start/installing-companion.html)，将地面站和树莓派IP地址配置到同一网段
- 如果采用路由器的话，自动分配IP，更方便，不需要自己配置 😂
- 确认地面站和树莓派处于同一网段后，打开/mavros/mavros/launch/px248.launch文件，将gcs_url的值改为地面站的IP地址
```
<!-- content of launch file --> 
<launch>
   <arg name="fcu_url" default="/dev/ttyACM0:921600" /> <!-- 921600:pixhawk communication frequency -->
   <arg name="gcs_url" default="udp://@192.168.8.181" /> <!-- turn url to IP on your PC -->
   <arg name="tgt_ststem" default="1" />
   <arg name="tgt_component" default="1" />
   <arg name="log_output" default="screen" />
   <arg name="fcu_protocol" default="v2.0" /> <!-- firmware version? -->
   <arg name="respawm_mavros" default="false" />
   
   <include file="$(find mavros)/launch/node.launch">
      <arg name="pluginlists_yaml" vaule="$(find mavros)/launch/px4_pluginlists.yaml" />
      <arg name="config_yaml" value="$(find mavros)/launch/px4_config.yaml" />
      <arg name="fcu_url" value="$(arg fcu_url)" />
      <arg name="gcs_url" value="$(arg gcs_url)" />
      <arg name="tgt_ststem" value="$(arg tgt_ststem)" />
      <arg name="tgt_component" value="$(arg tgt_component)" />
      <arg name="log_output" value="$(arg log_output)" />
      <arg name="fcu_protocol" value="$(arg fcu_protocol)" />
      <arg name="respawm_mavros" value="$(arg respawm_mavros)" />
   </include>
</launch>
```
- 将树莓派和飞控连接，执行下面代码，此时飞控可以与地面站通信 💝  
`roslaunch mavros px248.launch`

# 代码调试
## Code
`cout << "Hello World." << endl; // 这是单行代码`

```C++
// 这是代码块
void main(){
  cout << "Hello World." << endl;
  return ;
}
```

## 参考资料
- 【1】[树莓派实验室 Homepage](https://shumeipai.nxez.com/tag/%E6%A0%91%E8%8E%93%E6%B4%BE): Learn how to use raspberry pi.
- 【2】[ArduPilot Homepage](https://ardupilot.org/ardupilot/index.html): ArduPilot enables the creation and use of trusted, autonomous, unmanned vehicle systems for the peaceful benefit of all.
- 【3】[Ardusub gitbook](http://www.ardusub.com/): The ArduSub project is a fully-featured open-source solution for remotely operated underwater vehicles (ROVs) and autonomous underwater vehicles (AUVs).
- 【4】[BlueOS](https://docs.bluerobotics.com/ardusub-zola/software/onboard/BlueOS-1.0/): the lateset modified version of Ardusub
- 【5】[QGroundControl](https://docs.qgroundcontrol.com/master/en/): QGroundControl provides full flight control and vehicle setup for PX4 or ArduPilot powered vehicles. 
- 【6】[Robotics Homepage](https://bluerobotics.com/learn/)
- 【7】[BlueROV ROS wiki](http://wiki.ros.org/Robots/BlueROV): The BlueROV is a remotely operated underwater vehicle from Blue Robotics. It features Blue Robotics thrusters, a flight controller with ArduSub software, and a RaspberryPi running ROS.
- 【8】[ROVMaker Forum](http://rovmaker.cn/): Underwater Robotics Chinese Forum
