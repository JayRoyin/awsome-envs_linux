用DAPLink（OpenOCD）烧录 STM32 的完整指南
===

- [用DAPLink（OpenOCD）烧录 STM32 的完整指南](#用daplinkopenocd烧录-stm32-的完整指南)
  - [1. 下载 OpenOCD 源代码](#1-下载-openocd-源代码)
  - [2. 编译代码](#2-编译代码)
    - [2.1 运行 bootstrap](#21-运行-bootstrap)
    - [2.2 安装关联库](#22-安装关联库)
    - [2.3 运行 make](#23-运行-make)
    - [2.4 进行安装](#24-进行安装)
  - [3. 烧录程序](#3-烧录程序)
    - [3.1 准备配置文件](#31-准备配置文件)
    - [3.2 虚拟机 USB 设置（如使用虚拟机）](#32-虚拟机-usb-设置如使用虚拟机)
    - [3.3 启动 OpenOCD 服务器](#33-启动-openocd-服务器)
    - [3.4 连接 OpenOCD](#34-连接-openocd)
    - [3.5 烧录步骤](#35-烧录步骤)
      - [3.5.1 挂起 MCU](#351-挂起-mcu)
      - [3.5.2 写入镜像](#352-写入镜像)
      - [3.5.3 校验镜像](#353-校验镜像)
  - [4. 自动化脚本烧录](#4-自动化脚本烧录)
    - [4.1 创建脚本文件](#41-创建脚本文件)
    - [4.2 设置脚本权限](#42-设置脚本权限)
    - [4.3 运行脚本](#43-运行脚本)
  - [5. 常见问题解决](#5-常见问题解决)
    - [问题1：CMSIS-DAP 设备未找到](#问题1cmsis-dap-设备未找到)
    - [问题2：JTAG 不支持](#问题2jtag-不支持)
    - [问题3：权限问题](#问题3权限问题)
  - [注意事项](#注意事项)


***

## 1. 下载 OpenOCD 源代码

OpenOCD 官网：[Open On-Chip Debugger (openocd.org)](https://openocd.org/)

点击**Getting OpenOCD**，在 **Source Code** 字段找到[源代码链接](https://sourceforge.net/p/openocd/code/ci/master/tree/)（可能是**official Git source code repository**）。可以通过以下方式下载：

* 方法一：从 SourceForge 下载

    访问源代码网站，选择版本后点击 Download （这里选择的是**v0.12.0**版本），然后解压

    ![](/Embedded_Development/STM32/img/STM32_OpenOCD_Download.png)


* 方法二：使用 git 克隆（推荐）
    ```bash
    git clone https://git.code.sf.net/p/openocd/code openocd-code
    ```

## 2. 编译代码

进入下载的 OpenOCD 文件夹：
```bash
cd openocd-code
```

### 2.1 运行 bootstrap
```bash
./bootstrap
```

>[\!NOTE]
>**PKG_PROG_PKG_CONFIG 宏不可用**
>```
>configure.ac:32: error: Macro PKG_PROG_PKG_CONFIG is not available.
>It is usually defined in file pkg.m4 provided by package pkg-config.
>```
>**解决：**
>```bash
>sudo apt-get install pkg-config
>```


### 2.2 安装关联库

运行 `./configure` 检查缺少的库：
```bash
./configure
```

>[\!NOTE]
>进行OpenOCD配置，可配置的项目可以通过以下命令查看帮助信息。
>```bash
>./configure -h
>```

比如我的设备显示如下库的缺失
```bash
configure: error: jimtcl is required but not found via pkg-config and system includes
```
那么就根据配置输出安装缺失的库：
```bash
sudo apt install libjim-dev
```

>>[\!NOTE]
>由于我调试器是DAP-Link，所以还要运行如下命令进行使能
>```bash
>./configure --enable-cmsis-dap --enable-cmsis-dap-v2
>```
>```bash
>configure: error: hidapi is required for adapter_driver "CMSIS-DAP v1 compliant dongle (HID)".
>```
>```bash
>sudo apt install libhidapi-dev libusb-1.0-0-dev
>```

运行后`./configure`，显示OpenOCD的功能配置：

![](/Embedded_Development/STM32/img/STM_OpenOCD_configuration.png)


### 2.3 运行 make
```bash
make
```
会编译很长时间

### 2.4 进行安装
```bash
sudo make install
```
验证安装：
```bash
openocd -v
```
成功输出示例：
```
Open On-Chip Debugger 0.12.0+dev-02331-ge440b0648 (2025-12-25-09:54)
Licensed under GNU GPL v2
For bug reports, read
        http://openocd.org/doc/doxygen/bugs.html
```

## 3. 烧录程序

### 3.1 准备配置文件

1. 拷贝 CMSIS-DAP 配置文件到 hex 文件所在文件夹：
```bash
cp openocd-code/tcl/interface/cmsis-dap.cfg /path/to/hex/folder/
```

2. 拷贝 STM32F103C8 配置文件：
```bash
cp openocd-code/tcl/board/stm32f103c8_blue_pill.cfg /path/to/hex/folder/stm32f103c8.cfg
```

3. 编辑 `stm32f103c8.cfg` 文件，修改路径为当前路径：
```cfg
source [find stm32f1x.cfg]
```

### 3.2 虚拟机 USB 设置（如使用虚拟机）

1. 检查 USB 设备：
```bash
lsusb
```

2. 如果看不到 USB 设备，执行以下操作：
   - 安装 VirtualBox 增强功能
   - 在 VirtualBox 设置中选择 USB 3.0（如果设备使用 USB 3.0 接口）
   - 添加 USB 设备过滤器

### 3.3 启动 OpenOCD 服务器

在 hex 文件所在文件夹运行：
```bash
sudo openocd -f cmsis-dap.cfg -f stm32f103c8.cfg
```

成功启动的输出示例：
```
Info: auto-selecting first available session transport "swd". To override use 'transport select <transport>'.
Info: Listening on port 6666 for tcl connections
Info: Listening on port 4444 for telnet connections
Info: CMSIS-DAP: SWD supported
Info: CMSIS-DAP: Atomic commands supported
Info: CMSIS-DAP: FW Version = 2.0.0
Info: CMSIS-DAP: Interface Initialised (SWD)
Info: SWCLK/TCK = 1 SWDIO/TMS = 1 TDI = 0 TDO = 1 nTRST = 1 nRESET = 1
Info: CMSIS-DAP: Interface ready
Info: clock speed 1000 kHz
Info: SWD DPIDR 0x1ba01477
Info: [stm32f1x.cpu] Cortex-M3 r1p1 processor detected
Info: [stm32f1x.cpu] target has 6 breakpoints, 4 watchpoints
Info: starting gdb server for stm32f1x.cpu on 3333
Info: Listening on port 3333 for gdb connections
```

### 3.4 连接 OpenOCD

打开新的终端，连接到 OpenOCD：
```bash
telnet localhost 4444
```

连接成功后显示：
```
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
Open On-Chip Debugger
> 
```

### 3.5 烧录步骤

#### 3.5.1 挂起 MCU
```bash
> halt
```
输出：
```
[stm32f1x.cpu] halted due to debug-request, current mode: Thread 
xPSR: 0x81000000 pc: 0x0800046c msp: 0x20004ff8
```

#### 3.5.2 写入镜像
```bash
> flash write_image erase stm32f10x.hex
```
输出：
```
Adding extra erase range, 0x08004c6c .. 0x08004fff
auto erase enabled
wrote 19564 bytes from file stm32f10x.hex in 2.569553s (7.435 KiB/s)
```

#### 3.5.3 校验镜像
```bash
> flash verify_image stm32f10x.hex
```
输出：
```
verified 19564 bytes from file stm32f10x.hex in 0.124474s (153.490 KiB/s)
```

## 4. 自动化脚本烧录

### 4.1 创建脚本文件

创建 `program.sh` 文件：
```bash
#!/bin/bash

echo "OpenOCD-program stm32f103"

openocd -f cmsis-dap.cfg \
	-f stm32f103c8.cfg \
	-c "program stm32f10x.hex verify reset exit"
```

### 4.2 设置脚本权限
```bash
chmod +x program.sh
```

### 4.3 运行脚本
```bash
sudo ./program.sh
```

## 5. 常见问题解决

### 问题1：CMSIS-DAP 设备未找到
```
Error: unable to find a matching CMSIS-DAP device
```
**解决方案：**
1. 检查 USB 连接
2. 虚拟机用户检查 USB 设置
3. 运行 `lsusb` 确认设备识别

### 问题2：JTAG 不支持
```
Error: CMSIS-DAP: JTAG not supported
```
**解决方案：**
确保使用 SWD 模式，配置文件正确

### 问题3：权限问题
**解决方案：**
使用 `sudo` 运行 OpenOCD，或将用户添加到 dialout 组：
```bash
sudo usermod -a -G dialout $USER
```

## 注意事项

1. **配置文件路径**：确保所有配置文件都在同一目录或正确设置路径
2. **固件格式**：OpenOCD 支持多种格式，如 hex、bin、elf 等
3. **目标芯片**：根据实际芯片型号调整配置文件
4. **调试器类型**：本文使用 CMSIS-DAP，其他调试器需相应调整配置

---

**安装pyocd：**
```bash
pip3 install --user pyocd
```

>[\!NOTE]
>安装Python3和pip（如果尚未安装）：
>```bash
>sudo apt-get install python3 python3-pip
>```

https://blog.csdn.net/pq113_6/article/details/131771253
[OpenOCD - Open On-Chip Debugger Code ](https://sourceforge.net/p/openocd/code/ci/master/tree/)

**将pyocd添加到PATH：**
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

**安装CMSIS包（包含STM32F1系列支持）：**
```bash
pyocd pack install STM32F1
```

**验证pyocd安装：**
```bash
pyocd --version
```