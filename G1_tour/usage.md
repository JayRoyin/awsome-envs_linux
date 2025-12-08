# 导览机器人启动方法

# 1. 脚本一键启动（机器人本体启动）

### 准备工作
* 显示屏
* 键盘
* realsense深度摄像头

启动机器人，并连接上显示屏（连接到9号C口），进入桌面
  
### 启动脚本

首先进入工作空间`embodied_perception_sdk`
```bash
cd embodied_perception_sdk 
```

找到`robot_launch.sh`机器人启动脚本

```bash
./robot_launch.sh
# 进入机器人启动的脚本菜单
```
默认情况下，直接选择`1 ）完整启动流程`，等待脚本执行到自动打开新的终端并启动`roscore`后

切换回脚本所在的终端，等待机器人控制系统（交互式）出现后选择**y**后（**切记*不要回车***）
>如果按了回车，最简单的办法就是重启，其他办法请查看**FQA**）

等待运控终端启动后，记得切回脚本终端

接下来的三种模式
`damped模式`
`stand模式`
`walk模式`
中，有`stand模式`
`walk模式`都需要人扶着站起来，等机器人进入`walk模式`开始跺脚就可以松手。

然后脚本进入**机器人模式选择**的界面，回到主菜单继续启动**感知SDK**

不要修改场景名称，选择`realsense`摄像头，等待SDK启动。
>注意 启动肯定是有问题的，它会自动报错并且退出运行。

如果SDK启动失败则按方向键上寻找或则敲下列指令
```bash
roslaunch embodied_perception_sdk pipeline.launch use_gui:=false rgbd_type:=realsense slam_mode:=reloc slm_site_config:unitree-tairos.yaml launch_perception:=false scene_name:=guide_qsoffice_2025
```
下列是新版SDK的启动方式
```bash
# Note1: 将<my_scene>替换成当前场景的名字
# Note2: 将<platform_name>替换为当前机器人平台名字，目前支持`unitree_g1`, `dobot_hexplorer`, `astribot_s1`和`engineai_pm01`
#        以unitree_g1为例，最终使用`platform_name:=unitree_g1`
# Note3: 如果使用orbbec相机, 以下命令的参数修改为`rgbd_type:=orbbec`
# Note4: 如果暂时不适用rgbd相机的VQA功能，可以新增参数`launch_rgbd:=rgbd`
roslaunch embodied_perception_sdk pipeline.launch use_gui:=false rgbd_type:=realsense slam_mode:=reloc platform_name:=<platform_name> launch_perception:=false scene_name:=<my_scene>
```
操作遥控器让G1回到起始位置（扫图的初始点位）, 否则后续的重定位会失败。
```bash
# 如果出现下面的Log，说明算法还在尝试重定位，请耐心等待
Slam is abnormal, attempting relocalization once again..

Monitoring the lio's status, don't move it!

# 如果出现下面的Log，说明重定位成功，可以移动机器人 
Slam's status monitor finished, start moving!
```
语音模块成功运行后应该可以看到```log: G1 SDK ```导入成功 和 ```G1音频系统初始化成功```

# 2. 指令启动（机器人本体启动）

**updateing**

# FAQ
* 相机驱动报错`Failed to find device`或`No connected device found`

请确认当前使用的相机与指定的`rgbd_type`参数是否一致，或者是否连接上摄像头


---
[**返回主目录**](/README.md)
---