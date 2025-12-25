用 STLink 烧录 STM32 的完整指南
===
- [用 STLink 烧录 STM32 的完整指南](#用-stlink-烧录-stm32-的完整指南)
  - [系统环境：](#系统环境)
  - [一、安装依赖包:](#一安装依赖包)
  - [二、下载源码](#二下载源码)
  - [三、编译](#三编译)
  - [四、测试](#四测试)
  - [五、添加udev的rules规则](#五添加udev的rules规则)


---
 
## 系统环境：

Vmware12，

Ubuntu16.04

Stlink version：v1.4.0


## 一、安装依赖包:

 
```bash
sudo apt-get install libusb-1.0

sudo apt-get install cmake

sudo apt-get install libgtk-3-dev
```
 

## 二、下载源码
```bash
git clone https://github.com/texane/stlink.git
```

## 三、编译

在命令行终端中输入命令进行编译：
```bash
cd stlink && make release && make debug
```
```bash
cd build
```
```bash
cmake -DCMAKE_BUILD_TYPE=Debug ..
```
```bash
make
```
```bash
cd Release; sudo make install；sudo ldconfig
```
回到stlink目录下
```bash
cd  ../..  

sudo  cp  etc/udev/rules.d/*  /etc/udev/rules.d/

udevadm control --reload-rules

udevadm trigger
```
 

## 四、测试

在命令行终端中输入命令：
```bash
st-info –version
```
会看到类似如下提示:
```
v1.4.0
```
 

## 五、添加udev的rules规则

添加udev规则的目的是可以让应用程序可以访问STlink仿真器设备。

把STlink仿真器插到电脑的USB口，待Ubuntu系统识别后，在命令行终端中输入命令：```lsusb```
```bash
clip_image001
```
如上图所示，第二行可以看到STlink仿真器的类型和product ID和厂商ID。然后进入/etc/udev/rules.d/目录下，可以看到该目录下有一个99-vmware-scsi-udev.rules文件。在该文件中添加STlink设备信息，如下图第9行所示：
```
clip_image003
```