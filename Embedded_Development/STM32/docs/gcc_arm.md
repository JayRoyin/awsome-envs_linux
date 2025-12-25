GNU Arm Embedded Toolchain 安装与使用指南
===

- [GNU Arm Embedded Toolchain 安装与使用指南](#gnu-arm-embedded-toolchain-安装与使用指南)
  - [前言](#前言)
  - [一、GNU Arm Embedded Toolchain 简介](#一gnu-arm-embedded-toolchain-简介)
  - [二、gcc-arm-none-eabi 安装](#二gcc-arm-none-eabi-安装)
    - [2.1 自动安装](#21-自动安装)
    - [2.2 添加新的 PPA 进行安装](#22-添加新的-ppa-进行安装)
    - [2.3 手动安装（推荐）](#23-手动安装推荐)
  - [三、gcc-arm-none-eabi 移除](#三gcc-arm-none-eabi-移除)
    - [3.1 自动安装的卸载](#31-自动安装的卸载)
    - [3.2 手动安装的移除](#32-手动安装的移除)
  - [四、gcc-arm-none-eabi 版本切换](#四gcc-arm-none-eabi-版本切换)
    - [4.1 切换原理简介](#41-切换原理简介)
    - [4.2 切换步骤](#42-切换步骤)
  - [版权声明](#版权声明)

---

## 前言
在 Ubuntu 中开发基于 ARM 架构的 STM32 芯片，需要安装交叉编译器 gcc-arm-none-eabi 编译代码。

## 一、GNU Arm Embedded Toolchain 简介
GNU Arm Embedded Toolchain 是用于 C、C++ 和汇编编程的即用型开源工具套件。

## 二、gcc-arm-none-eabi 安装

### 2.1 自动安装
**网络环境较差时不推荐**

```bash
sudo apt-get install gcc-arm-none-eabi
```

```bash
arm-none-eabi-gcc -v
```

### 2.2 添加新的 PPA 进行安装
**网络环境较差时不推荐**

```bash
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
```

```bash
sudo apt-get update
```

```bash
sudo apt-get install gcc-arm-embedded
```

**如果需要移除已安装的工具链：**
```bash
sudo apt-get remove gnu-arm-embedded
```

**如果报告冲突错误（从 4.x 升级到 5+）：**
```bash
sudo apt-get remove gcc-arm-none-eabi
```

```bash
sudo apt-get install gcc-arm-none-eabi
```

### 2.3 手动安装（推荐）

[**官网下载**](https://developer.arm.com/downloads/-/gnu-rm)

这里选择了 `6-2017-q2-update`，可以根据自己的需要下载其它版本。 

进入软件包下载目录，打开终端，输入命令：
（1）改变当前目录到指定目录，并保存当前的目录在堆栈顶端；

```bash
pushd .
```

（2）解压下载的软件包（等待解压完成，生成新的文件夹，用 ls 命令可以查看）；
```bash
tar -jxf gcc-arm-none-eabi-6-2017-q2-update-linux.tar.bz2
```

（3）将解压出来的软件包（gcc-arm-none-eabi-6-2017-q2-update）移动到 Ubuntu 根目录下的 opt 文件夹中；
```bash
sudo mv gcc-arm-none-eabi-6-2017-q2-update /opt
```

（4）添加环境变量到用户目录下的 .profile 文件（该文件是隐藏文件，在用户目录界面中通过 Ctrl + h 快捷方式可以显示隐藏文件）；
```bash
exportline="export PATH=/opt/gcc-arm-none-eabi-6-2017-q2-update/bin:\$PATH"
```

（5）检测环境变量是否添加成功；
```bash
if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
```

添加成功不会有输出；如果添加失败，会输出提示。

（6）使能环境变量；
```bash
source ~/.profile
```

（7）改变当前目录，跳转到堆栈顶端保存的目录，并将堆栈顶端的目录删除；
```bash
popd
```

注意：如果安装其他版本的 gcc 编译器，在命令（2）、（3）、（4）中，要把压缩包名和解压的文件名做相应替换。

（8）检查交叉编译器是否安装成功，输入以下命令；
```bash
arm-none-eabi-gcc -v
```


输入命令后，最后一行打印了我们刚才安装的交叉编译器的版本信息，表示安装成功。

## 三、gcc-arm-none-eabi 移除

### 3.1 自动安装的卸载

```bash
sudo apt-get remove gcc-arm-none-eabi
```

### 3.2 手动安装的移除

```bash
sudo rm -rf /opt/gcc-arm-none-eabi-6-2017-q2-update/
```

```bash
sudo gedit .profile
```

**编辑 .profile 文件，删除或屏蔽编译器安装路径**

```bash
source ~/.profile
```

## 四、gcc-arm-none-eabi 版本切换

### 4.1 切换原理简介
通过修改环境变量文件 `.profile` 中的安装包路径来控制使用的编译器版本。

### 4.2 切换步骤

```bash
arm-none-eabi-gcc -v
```

```bash
sudo gedit .profile
```

**编辑 .profile 文件：**
- 屏蔽不需要的版本路径（在路径前加 `#`）
- 保留需要的版本路径

```bash
source ~/.profile
```

```bash
arm-none-eabi-gcc -v
```

**如果版本切换失败：**
```bash
reboot
```

---

## 版权声明

本文中参考或引用的部分内容遵循 **CC 4.0 BY-SA** 版权协议。若需转载，请附上原文出处链接及以下相关声明：
- [**博主「EmotionFlying」的原创文章**](https://blog.csdn.net/qq_20016593/article/details/125343260)
   遵循 CC 4.0 BY-SA 版权协议。

- [**博主「pq113_6」的原创文章**](https://blog.csdn.net/pq113_6/article/details/131771253)
   遵循 CC 4.0 BY-SA 版权协议。

- [**博主「猫头鹰黑」的原创文章**](https://blog.csdn.net/weixin_42415539/article/details/99873451)
   遵循 CC 4.0 BY-SA 版权协议。
