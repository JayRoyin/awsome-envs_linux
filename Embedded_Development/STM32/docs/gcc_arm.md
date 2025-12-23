GNU Arm Embedded Toolchain 安装与使用指南
===

- [GNU Arm Embedded Toolchain 安装与使用指南](#gnu-arm-embedded-toolchain-安装与使用指南)
  - [前言](#前言)
  - [一、GNU Arm Embedded Toolchain 简介](#一gnu-arm-embedded-toolchain-简介)
  - [二、gcc-arm-none-eabi 安装](#二gcc-arm-none-eabi-安装)
    - [2.1 自动安装](#21-自动安装)
    - [2.2 添加新的 PPA 进行安装](#22-添加新的-ppa-进行安装)
    - [2.3 手动安装（推荐）](#23-手动安装推荐)
      - [2.3.1 软件下载](#231-软件下载)
      - [2.3.2 安装步骤](#232-安装步骤)
  - [三、gcc-arm-none-eabi 移除](#三gcc-arm-none-eabi-移除)
    - [3.1 自动安装的卸载](#31-自动安装的卸载)
    - [3.2 手动安装的移除](#32-手动安装的移除)
  - [四、gcc-arm-none-eabi 版本切换](#四gcc-arm-none-eabi-版本切换)
    - [4.1 切换原理简介](#41-切换原理简介)
    - [4.2 切换步骤](#42-切换步骤)
  - [总结](#总结)


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

#### 2.3.1 软件下载
[官网](https://developer.arm.com/downloads/-/gnu-rm)

#### 2.3.2 安装步骤

```bash
pushd .
```

```bash
tar -jxf gcc-arm-none-eabi-6-2017-q2-update-linux.tar.bz2
```

```bash
sudo mv gcc-arm-none-eabi-6-2017-q2-update /opt
```

```bash
exportline="export PATH=/opt/gcc-arm-none-eabi-6-2017-q2-update/bin:\$PATH"
```

```bash
if grep -Fxq "$exportline" ~/.profile; then echo nothing to do ; else echo $exportline >> ~/.profile; fi
```

```bash
source ~/.profile
```

```bash
popd
```

```bash
arm-none-eabi-gcc -v
```

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

## 总结
推荐使用手动安装方法，方便后续版本管理和版本切换。通过修改环境变量可以方便快捷地切换不同版本的交叉编译器。