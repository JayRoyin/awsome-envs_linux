# 嵌入式开发环境配置指南

欢迎来到嵌入式开发环境配置仓库！这里提供了STM32和ESP32两大主流嵌入式平台的开发环境配置指南。

## 📁 项目结构

```
.
├── ESP32/              # ESP32开发环境配置
│   ├── README.md       # ESP32环境配置主文档
│   ├── docs/           # ESP32相关文档
│   └── img/            # ESP32配置截图
├── STM32/              # STM32开发环境配置
│   ├── README.md       # STM32环境配置主文档
│   ├── docs/           # STM32相关文档
│   └── img/            # STM32配置截图
└── README.md           # 本文件（项目总览）
```

## 🚀 快速导航

### [ESP32 开发环境配置](ESP32/README.md)
- ESP-IDF开发框架安装
- VSCode环境配置
- 串口权限设置
- 常见问题解答

### [STM32 开发环境配置](STM32/README.md)
- ARM GCC工具链安装
- OpenOCD调试配置
- STM32CubeMX使用指南
- Makefile项目构建

## 📋 环境要求

- **操作系统**: Ubuntu 20.04 LTS 或 Ubuntu 22.04
- **内存**: 至少 4GB RAM
- **存储**: 至少 10GB 可用空间
- **网络**: 需要连接互联网以下载工具链和依赖包

## 🔧 通用工具

以下工具在两个平台开发中都会用到：

1. **Git** - 版本控制
2. **VSCode** - 代码编辑器
3. **串口调试工具** - minicom/screen
4. **Make** - 项目构建工具

## 🤝 贡献指南

如果您发现文档中有任何错误或需要补充的内容，欢迎提交Issue或Pull Request！
