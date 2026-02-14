<div align="center">

# 🐧 Awesome Envs Linux

**Ubuntu 开发环境配置百科 · 从零到一的完整指南**

[![OS](https://img.shields.io/badge/OS-Ubuntu_22.04+-E95420?style=flat-square&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![License](https://img.shields.io/badge/License-MIT-blue?style=flat-square)](LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen?style=flat-square)](https://github.com)

> 记录和整理各种开发环境、工具链和库的配置过程、使用指南以及常见问题解决方案。
> 无论你是搞嵌入式、深度学习、游戏开发还是日常 Linux 运维，都能在这里找到参考。

</div>

---

## 📑 目录

- [嵌入式开发](#-嵌入式开发)
- [GPU / 深度学习环境](#-gpu--深度学习环境)
- [Docker](#-docker)
- [编译器 & 工具链](#%EF%B8%8F-编译器--工具链)
- [版本控制](#-版本控制)
- [游戏引擎](#-游戏引擎)
- [网络工具](#-网络工具)
- [机器人 & SDK](#-机器人--sdk)
- [项目结构](#-项目结构)

---

## 🔌 嵌入式开发

<table>
<tr><td colspan="2"><b>ESP32 系列</b> — 基于 ESP-IDF 5.5 的开发环境搭建 & 小智 AI 项目实战</td></tr>
<tr>
  <td>📖 <a href="Embedded_Development/ESP32/README.md">ESP32 开发环境搭建（主文档）</a></td>
  <td>ESP-IDF 安装、环境变量、HelloWorld、小智 AI 工程全流程</td>
</tr>
<tr>
  <td>📄 <a href="Embedded_Development/ESP32/docs/port_permission.md">串口权限问题</a></td>
  <td>Permission denied 排查与修复</td>
</tr>
<tr>
  <td>📄 <a href="Embedded_Development/ESP32/docs/FAQ.md">常见问题 FAQ</a></td>
  <td>串口连接、编译报错等故障排除</td>
</tr>
</table>

<table>
<tr><td colspan="2"><b>STM32 系列</b> — Ubuntu 下从零搭建 STM32 开发环境到点亮 LED</td></tr>
<tr>
  <td>📖 <a href="Embedded_Development/STM32/README.md">STM32 开发环境搭建（主文档）</a></td>
  <td>gcc-arm + pyocd + STM32CubeMX 完整流程</td>
</tr>
<tr>
  <td>📄 <a href="Embedded_Development/STM32/docs/gcc_arm.md">ARM GCC 工具链手动安装</a></td>
  <td>GNU Arm Embedded Toolchain 安装与版本管理</td>
</tr>
<tr>
  <td>📄 <a href="Embedded_Development/STM32/docs/DAPLink.md">DAPLink (OpenOCD) 烧录指南</a></td>
  <td>OpenOCD 编译安装 + DAPLink 烧录配置</td>
</tr>
<tr>
  <td>📄 <a href="Embedded_Development/STM32/docs/STLink.md">STLink 烧录指南</a></td>
  <td>stlink-tools 安装与使用</td>
</tr>
<tr>
  <td>📄 <a href="Embedded_Development/STM32/docs/STM32CubeProg_install.md">STM32CubeProgrammer 安装</a></td>
  <td>STM32CubeProg Linux 版安装步骤</td>
</tr>
</table>

---

## 🎮 GPU / 深度学习环境

| 文档 | 说明 |
|:-----|:-----|
| 🟢 [NVIDIA 驱动安装](Nvidia_env/nvidia_drive.md) | Ubuntu 22.04–24.04 显卡驱动安装与切换 |
| 🟢 [CUDA 版本管理](Nvidia_env/cuda_version.md) | CUDA Toolkit 安装、多版本共存与切换 |
| 🟢 [Conda 环境配置](Nvidia_env/conda_config.md) | Anaconda / Miniconda 安装、虚拟环境管理 |
| 🟢 [libstdc++ 兼容修复](Nvidia_env/libstdc.md) | `libstdc++.so.6` 版本升级与符号链接修复 |

---

## 🐳 Docker

| 文档 | 说明 |
|:-----|:-----|
| 📦 [Docker 安装指南](docker_config/docker_install.md) | Ubuntu 上通过 apt 安装 Docker Engine |
| 📦 [Docker 超时问题排查](docker_config/timeout.md) | `hello-world` 镜像拉取失败 / 超时的解决方案 |

---

## ⚙️ 编译器 & 工具链

| 文档 | 说明 |
|:-----|:-----|
| 🔧 [GCC / G++ 版本切换](gcc/g++.md) | `update-alternatives` 多版本管理 |

---

## 🌿 版本控制

| 文档 | 说明 |
|:-----|:-----|
| 📝 [Git 基础使用指南](git_usage/git.md) | 从本地仓库到 GitHub 远程推送的完整流程 |

---

## 🎮 游戏引擎

| 文档 | 说明 |
|:-----|:-----|
| 🕹️ [Unity Hub Linux 安装](unity/unity_linux_install.md) | UnityHub 安装、编辑器下载与常见问题 |
| 🕹️ [Unity 组件手动安装](unity/Component_install.md) | Android Build Support、JDK、Gradle 等组件安装 |

---

## 🌐 网络工具

| 文档 | 说明 |
|:-----|:-----|
| 🛡️ [Clash 自定义规则配置](docs/Spec/clash.md) | 订阅更新不覆盖自定义分流规则的方法 |

---

## 🤖 机器人 & SDK

| 文档 | 说明 |
|:-----|:-----|
| 🦾 [G1 导览机器人启动指南](G1_tour/usage.md) | 脚本一键启动、远程开发流程 |
| 🦾 [Unitree SDK 示例运行](study/示例.md) | G1 高层运动控制 SDK 编译与运行 |

---

## 🗂 项目结构

```
awsome-envs_linux/
├── README.md                          # ← 你在这里
├── Embedded_Development/              # 嵌入式开发
│   ├── ESP32/                         #   ├── ESP-IDF & 小智 AI
│   │   ├── README.md
│   │   ├── docs/                      #   │   FAQ、串口权限
│   │   └── img/
│   └── STM32/                         #   └── ARM GCC & pyocd
│       ├── README.md
│       ├── docs/                      #       gcc_arm、DAPLink、STLink、CubeProg
│       └── img/
├── Nvidia_env/                        # GPU 环境：驱动、CUDA、Conda、libstdc++
├── docker_config/                     # Docker 安装 & 超时排查
├── gcc/                               # GCC/G++ 版本切换
├── git_usage/                         # Git 使用指南
├── unity/                             # Unity Hub & 组件安装
├── docs/Spec/                         # 网络工具（Clash 规则）
├── G1_tour/                           # G1 导览机器人
├── study/                             # SDK 示例 & 学习笔记
└── test/                              # C/C++ 测试代码
```

---

<div align="center">

如果这些文档对你有帮助，欢迎 ⭐ Star 支持一下！

有问题或补充？欢迎提 [Issue](../../issues) 或 [Pull Request](../../pulls)。

</div>