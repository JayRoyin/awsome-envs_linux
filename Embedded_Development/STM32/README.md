Ubuntu下STM32开发环境搭建指南（从零开始到点亮小灯）
===

- [Ubuntu下STM32开发环境搭建指南（从零开始到点亮小灯）](#ubuntu下stm32开发环境搭建指南从零开始到点亮小灯)
  - [一、开发环境搭建思路](#一开发环境搭建思路)
  - [二、开发工具准备](#二开发工具准备)
    - [系统平台](#系统平台)
    - [软件工具](#软件工具)
    - [硬件平台](#硬件平台)
  - [三、详细配置过程](#三详细配置过程)
    - [3.1 安装串口调试助手](#31-安装串口调试助手)
    - [3.2 安装交叉编译器gcc-arm-none-eabi](#32-安装交叉编译器gcc-arm-none-eabi)
    - [3.3 安装下载工具](#33-安装下载工具)
    - [3.4 安装编译工具](#34-安装编译工具)
    - [3.5 安装STM32CubeMX](#35-安装stm32cubemx)
    - [3.6 安装VSCode（可选但推荐）](#36-安装vscode可选但推荐)
  - [四、创建STM32项目](#四创建stm32项目)
    - [4.1 使用STM32CubeMX生成项目](#41-使用stm32cubemx生成项目)
    - [4.2 编写LED闪烁程序](#42-编写led闪烁程序)
  - [五、编译项目](#五编译项目)
    - [5.1 进入项目目录](#51-进入项目目录)
    - [5.2 编译项目](#52-编译项目)
  - [六、下载程序到STM32](#六下载程序到stm32)
    - [6.1 连接硬件](#61-连接硬件)
    - [6.2 使用pyocd下载程序](#62-使用pyocd下载程序)
    - [6.3 验证程序运行](#63-验证程序运行)
  - [七、调试配置（可选）](#七调试配置可选)
    - [7.1 安装GDB调试器](#71-安装gdb调试器)
    - [7.2 配置VSCode调试](#72-配置vscode调试)
  - [八、常见问题解决](#八常见问题解决)
    - [8.1 pyocd找不到设备](#81-pyocd找不到设备)
    - [8.2 编译错误](#82-编译错误)
    - [8.3 串口无法打开](#83-串口无法打开)
  - [九、自动化脚本](#九自动化脚本)
    - [9.1 创建一键编译下载脚本](#91-创建一键编译下载脚本)
  - [十、总结](#十总结)

---

## 一、开发环境搭建思路

在Ubuntu下开发STM32的总体思路：
1. 使用Makefile或CMake将STM32的库文件代码和用户代码组织起来
2. 使用gcc-arm-none-eabi交叉编译器进行编译链接
3. 使用DAP下载器将生成的二进制文件下载到STM32上

## 二、开发工具准备

### 系统平台
**系统版本**：Ubuntu 22.04

### 软件工具
**串口调试助手**：minicom或cutecom
**交叉编译器**：gcc-arm-none-eabi
**编译工具**：cmake或make
**下载工具**：pyocd（支持DAP）
**代码配置工具**：STM32CubeMX
**代码编辑器**：VSCode

### 硬件平台
STM32开发板（本文以STM32F103C6T6为例）
DAP下载器

## 三、详细配置过程

### 3.1 安装串口调试助手

下面有俩个推荐的串口调试工具：**minicom**和**cutecom**，**minicom**是基于命令行的，而**cutecom**是图形界面（cutecom使用起来比较简单，跟windows的的串口助手一样）。

**安装minicom（命令行界面）：**
```bash
sudo apt-get update
sudo apt-get install minicom
```

**配置minicom：**
```bash
sudo minicom -s
```
>配置说明：
>- 选择"Serial port setup"
>- 设置串口设备（如：/dev/ttyUSB0或则ttyACM0）
>- 设置波特率（通常为115200或则9600）
>- 保存配置为默认配置

**安装cutecom（图形界面）：**
```bash
sudo apt-get install cutecom
```

**运行cutecom：**
```bash
cutecom
```

>[\!NOTE]
>cutecom有桌面图标不用自己配置，而minicom必须用命令启动

### 3.2 安装交叉编译器gcc-arm-none-eabi

**方法一：自动安装（简单但不一定是最新版）**
```bash
sudo apt-get update
sudo apt-get install gcc-arm-none-eabi
```

**方法二：手动安装（推荐，可控制版本）**

可以先去[官网](https://developer.arm.com/downloads/-/gnu-rm)查看适合自己的编译器，这里我选择的是[`gcc-arm-none-eabi-10.3-2021.10-x86_64-linux.tar.bz2`](https://developer.arm.com/-/media/files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-x86_64-linux.tar.bz2?rev=78196d3461ba4c9089a67b5f33edf82a&revision=78196d34-61ba-4c90-89a6-7b5f33edf82a&hash=B94A380A17942218223CD08320496FB1)

具体手动安装教程可以查看[gcc-arm手动安装教程](/Embedded_Development/STM32/docs/gcc_arm.md)

>[\!NOTE]
>有能力的人在配置环境的时候可以将该切换版本的方法和conda融合在一起使用

**验证安装：**
```bash
arm-none-eabi-gcc --version
```
输出类似于：
```bash
gcc version 10.3.1 20210621 (release) (15:10.3-2021.07-4)
```

STM32CubeProgrammer

### 3.3 安装下载工具
这里提供三种下载工具的方法DAPLink、STLink和STM32cubeProgrammer，其中STM32cubeProgrammer功能最为强大，但是安装起来也很麻烦需要JAVA环境

- **STM32cubeProgrammer**:
支持格式：Motorola S19、ELF、bin、hex


- **DAPLink**：简化了将调试器连接到基于ARM Cortex的目标板上，具体可以到官网查看[CMSIS-DAP](https://developer.arm.com/documentation/101451/0100/About-CMSIS-DAP)
  

- **STLink**：仅适用于ST 芯片，尤其是STM32和STM8。
  - [ST-LINK](https://www.st.com/en/development-tools/st-link-v2.html)
  - [ST-LINK/V2](https://www.st.com/en/development-tools/st-link-v2.html)

### 3.4 安装编译工具

**安装make和cmake：**
```bash
sudo apt-get install make cmake build-essential
```

### 3.5 安装STM32CubeMX

1. **下载STM32CubeMX：**
前往ST官网下载Linux版本：https://www.st.com/en/development-tools/stm32cubemx.html

2. **解压安装包：**
```bash
unzip en.stm32cubemx-lin.zip
```

3. **安装Java运行环境：**
```bash
sudo apt-get install default-jre
```

4. **安装STM32CubeMX：**
```bash
sudo ./SetupSTM32CubeMX-6.5.0.linux
```

5. **创建桌面快捷方式：**
```bash
sudo nano /usr/share/applications/STM32CubeMX.desktop
```
添加以下内容：
```
[Desktop Entry]
Name=STM32CubeMX
Comment=STM32CubeMX Configuration Tool
Exec=/usr/local/STMicroelectronics/STM32Cube/STM32CubeMX/STM32CubeMX
Icon=/usr/local/STMicroelectronics/STM32Cube/STM32CubeMX/help/STM32CubeMX.ico
Terminal=false
Type=Application
Categories=Development;
```

### 3.6 安装VSCode（可选但推荐）

```bash
sudo apt-get install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt-get update
sudo apt-get install code
```

**安装VSCode插件：**
1. C/C++
2. Cortex-Debug
3. CMake Tools

## 四、创建STM32项目

### 4.1 使用STM32CubeMX生成项目

1. **启动STM32CubeMX：**
```bash
STM32CubeMX
```

2. **创建新项目：**
   - 选择芯片型号：STM32F103VETx
   - 配置系统时钟、GPIO等

3. **配置项目设置：**
   - 项目名称：LED_Blink
   - 项目路径：选择合适的路径
   - Toolchain/IDE：选择"Makefile"

4. **配置GPIO（以PB0为例）：**
   - 点击PB0引脚，选择"GPIO_Output"
   - 在左侧配置GPIO输出模式

5. **生成代码：**
   - 点击"Generate Code"

### 4.2 编写LED闪烁程序

在生成的项目中找到`Src/main.c`文件，修改主循环：

```c
/* 在main函数的while循环中添加以下代码 */
while (1)
{
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
    HAL_GPIO_TogglePin(GPIOB, GPIO_PIN_0);
    HAL_Delay(1000);
    /* USER CODE END 3 */
}
```

## 五、编译项目

### 5.1 进入项目目录

```bash
cd ~/STM32CubeMX/LED_Blink
```

### 5.2 编译项目

**使用make编译：**
```bash
make clean
make all
```

**编译成功后会看到：**
```
arm-none-eabi-size build/LED_Blink.elf
   text    data     bss     dec     hex filename
   xxxx    xxxx    xxxx   xxxx    xxxx build/LED_Blink.elf
```

**检查生成的文件：**
```bash
ls build/
```
应该能看到以下文件：
- LED_Blink.elf
- LED_Blink.bin
- LED_Blink.hex

## 六、下载程序到STM32

### 6.1 连接硬件
1. 将DAP下载器连接到开发板
2. DAP的SWD接口连接到STM32的SWD引脚
3. 开发板上电

### 6.2 使用pyocd下载程序

**查看连接的设备：**
```bash
pyocd list
```
输出应类似：
```
  #   Probe              Unique ID
  0   CMSIS-DAP v1       xxxxxxxxxxxxxxxx [STLink]
```

**擦除芯片：**
```bash
pyocd erase --chip
```

**下载bin文件：**
```bash
pyocd load -t stm32f103vetx build/LED_Blink.bin
```

**下载hex文件：**
```bash
pyocd load -t stm32f103vetx build/LED_Blink.hex
```

**下载并运行：**
```bash
pyocd load -t stm32f103vetx build/LED_Blink.elf --start-addr 0x8000000
```

### 6.3 验证程序运行
下载完成后，开发板上的LED（连接到PB0）应该开始以1秒的间隔闪烁。

## 七、调试配置（可选）

### 7.1 安装GDB调试器

```bash
sudo apt-get install gdb-multiarch
```

### 7.2 配置VSCode调试

创建`.vscode/launch.json`：

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Cortex Debug",
            "cwd": "${workspaceRoot}",
            "executable": "./build/LED_Blink.elf",
            "request": "launch",
            "type": "cortex-debug",
            "servertype": "pyocd",
            "device": "STM32F103VE",
            "runToMain": true,
            "showDevDebugOutput": "raw"
        }
    ]
}
```

## 八、常见问题解决

### 8.1 pyocd找不到设备
```bash
# 查看USB设备
lsusb
# 检查用户权限
sudo usermod -a -G dialout $USER
# 重新登录或重启
```

### 8.2 编译错误
```bash
# 清理项目并重新编译
make clean
make all
```

### 8.3 串口无法打开
```bash
# 查看串口权限
ls -l /dev/ttyUSB*
# 添加用户到dialout组
sudo usermod -a -G dialout $USER
```

## 九、自动化脚本

### 9.1 创建一键编译下载脚本

创建`flash.sh`：

```bash
#!/bin/bash
echo "Cleaning project..."
make clean

echo "Building project..."
make all

if [ $? -eq 0 ]; then
    echo "Build successful!"
    echo "Flashing to device..."
    pyocd load -t stm32f103vetx build/LED_Blink.bin
else
    echo "Build failed!"
fi
```

**给脚本执行权限：**
```bash
chmod +x flash.sh
```

**运行脚本：**
```bash
./flash.sh
```

## 十、总结

本文详细介绍了在Ubuntu系统下搭建STM32开发环境的完整流程：
1. **环境搭建**：安装必要的开发工具
2. **项目创建**：使用STM32CubeMX配置和生成项目
3. **代码编写**：编写简单的LED闪烁程序
4. **编译构建**：使用make编译项目
5. **程序下载**：使用pyocd通过DAP下载器烧录程序
6. **验证调试**：验证程序运行和基本调试

此环境搭建方案具有以下优点：
- 完全开源免费
- 跨平台兼容性好
- 支持多种调试器（DAP、J-Link、ST-Link等）
- 自动化程度高，提高开发效率