NVIDIA Dirve安装
===
- **Ubuntu版本：** Ubuntu 22.04 - 24.04

- **Ubuntu内核：** 6.5~6.8（其他内核方法一样，只是驱动版本差异）

- **显卡驱动版本：** nvidia-driver-580（示例）


## 注意事项
在多设备下安装时发现，带有`UEFI-安全启动`的设备，会出现安装过程不报错，
但是执行 `nvidia-smi` 检查驱动时输出错误，该情况下需要开机时进入BIOS，关闭安全启动或清除安全启动密钥,
如果根据步骤安装显卡驱动成功，在执行`nvidia-smi`时出错，请自行甄别是否为安全启动的问题,
或者检查是否存在冲突，导致系统启动时驱动没有正常加载。

## 安装前置

1.在安装新驱动程序之前，建议卸载任何现有的 NVIDIA 驱动程序，避免冲突，命令如下。
```bash
sudo apt remove --purge '^nvidia-.*'
```
2.如果之前你已启用 `Nouveau` 驱动，可能需要将其禁用，连续执行以下两条命令：
```bash
sudo bash -c "echo blacklist nouveau > /etc/modprobe.d/blacklist-nvidia-nouveau.conf"

sudo bash -c "echo options nouveau modeset=0 >> /etc/modprobe.d/blacklist-nvidia-nouveau.conf"
```
3.确认是否禁用 `Nouveau` 驱动成功，然后更新 `initramfs`
```bash
cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

sudo update-initramfs -u
```
正确禁用输出
```bash
blacklist nouveau

options nouveau modeset=0
```

## 驱动安装开始

1.更新APT包和升级已安装软件包（ `sudo apt upgrade` 可选）。
```bash
sudo apt update

sudo apt upgrade
```
2.添加 NVIDIA 官方 PPA，一定要确保系统已经成功添加 `graphics-drivers PPA` 存储库。
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
```
正确添加输出

3.检查`graphics-drivers PPA` 存储库：
```bash
sudo apt list nvidia-driver*
```
正确输出
```bash
nvidia-driver-570-open/jammy-updates,jammy-security,jammy 570.195.03-0ubuntu0.22.04.1 amd64
nvidia-driver-570-server-open/jammy-updates,jammy-security 570.195.03-0ubuntu0.22.04.2 amd64
nvidia-driver-570-server/jammy-updates,jammy-security 570.195.03-0ubuntu0.22.04.2 amd64
nvidia-driver-570/jammy-updates,jammy-security,jammy 570.195.03-0ubuntu0.22.04.1 amd64
nvidia-driver-575-open/jammy-updates,jammy-security 575.64.03-0ubuntu0.22.04.3 amd64
nvidia-driver-575-server-open/jammy-updates,jammy-security 575.57.08-0ubuntu0.22.04.3 amd64
nvidia-driver-575-server/jammy-updates,jammy-security 575.57.08-0ubuntu0.22.04.3 amd64
nvidia-driver-575/jammy-updates,jammy-security 575.64.03-0ubuntu0.22.04.3 amd64
nvidia-driver-580-open/jammy-updates,jammy-security,jammy,now 580.95.05-0ubuntu0.22.04.1 amd64 [已安装]
nvidia-driver-580-server-open/jammy-updates,jammy-security 580.95.05-0ubuntu0.22.04.2 amd64
nvidia-driver-580-server/jammy-updates,jammy-security 580.95.05-0ubuntu0.22.04.2 amd64
nvidia-driver-580/jammy-updates,jammy-security,jammy 580.95.05-0ubuntu0.22.04.1 amd64
```
4.查看当前设备支持的驱动版本，驱动程序版本决定后续的CUDA驱动版本，建议高版本。
```bash
ubuntu-drivers devices
```
正确输出

5.安装驱动程序，这里以nvidia-driver-580版本为例。可自行查看显卡驱动版本支持的CUDA工具包：`NVIDIA CUDA Toolkit Release Notes`
>最好是安装open版本的,不然可能会出现`no device were found`
>Ubuntu只认Open开源版本的驱动，
```bash
sudo apt install nvidia-driver-580
# 或者
sudo apt install nvidia-driver-580-open
```
6.重启系统。
```bash
sudo reboot
```

7.验证驱动安装
```bash
nvidia-smi
```
正确输出如下
```bash
+-----------------------------------------------------------------------------------------+
| NVIDIA-SMI 580.95.05              Driver Version: 580.95.05      CUDA Version: 13.0     |
+-----------------------------------------+------------------------+----------------------+
| GPU  Name                 Persistence-M | Bus-Id          Disp.A | Volatile Uncorr. ECC |
| Fan  Temp   Perf          Pwr:Usage/Cap |           Memory-Usage | GPU-Util  Compute M. |
|                                         |                        |               MIG M. |
|=========================================+========================+======================|
|   0  NVIDIA GeForce RTX 5070 ...    Off |   00000000:02:00.0  On |                  N/A |
| N/A   36C    P8              5W /   50W |     477MiB /   8151MiB |      0%      Default |
|                                         |                        |                  N/A |
+-----------------------------------------+------------------------+----------------------+

+-----------------------------------------------------------------------------------------+
| Processes:                                                                              |
|  GPU   GI   CI              PID   Type   Process name                        GPU Memory |
|        ID   ID                                                               Usage      |
|=========================================================================================|
|    0   N/A  N/A            1382      G   /usr/lib/xorg/Xorg                      259MiB |
|    0   N/A  N/A            1644      G   /usr/bin/gnome-shell                     47MiB |
|    0   N/A  N/A            4211      G   .../4848/usr/lib/firefox/firefox         19MiB |
|    0   N/A  N/A            5785      G   /proc/self/exe                           79MiB |
+-----------------------------------------------------------------------------------------+
```

其中`CUDA Version: 13.0`是显卡所能支持的最高版本。
## CUDA安装
完成后继续[CUDA安装](cuda_version.md)