# Unitree 机器人 WiFi 连接问题排查与解决

## 问题描述

Unitree 机器人通过有线网络连接，无法连接 WiFi。

---

## 问题原因

排查发现两个关键问题：

### 1. WiFi 被 RF-kill 软锁定

```bash
$ rfkill list
0: hci0: Bluetooth
    Soft blocked: no
    Hard blocked: no
1: phy0: Wireless LAN
    Soft blocked: yes    # <-- WiFi 被软锁定
    Hard blocked: no
```

**原因**：Linux 系统的 RF-kill 机制禁用了无线设备，导致 WiFi 无法使用。

---

### 2. NetworkManager 配置错误

```ini
# /etc/NetworkManager/NetworkManager.conf
[ifupdown]
managed=false    # <-- NetworkManager 无法管理网络设备
```

**原因**：`managed=false` 导致 NetworkManager 无法管理 WiFi 设备，即使手动启用接口也无法连接网络。

---

## 解决方案

### 步骤 1：解除 RF-kill 软锁定

```bash
# 查看 RF-kill 状态
rfkill list

# 解除 WiFi 软锁定
sudo rfkill unblock wifi
```

---

### 步骤 2：修改 NetworkManager 配置

```bash
# 编辑 NetworkManager 配置文件
sudo sed -i 's/managed=false/managed=true/' /etc/NetworkManager/NetworkManager.conf

# 重启 NetworkManager 服务
sudo systemctl restart NetworkManager
```

---

### 步骤 3：启用 WiFi 并连接

```bash
# 启用 WiFi 接口
sudo ip link set wlan0 up

# 查看可用的 WiFi 网络
nmcli device wifi list

# 连接 WiFi（将 SSID 和密码替换为实际值）
sudo nmcli device wifi connect 'SSID' password 'PASSWORD' ifname wlan0

# 查看连接状态
nmcli device status
```

---

## 本次操作记录

| 操作 | 命令 | 结果 |
|------|------|------|
| 解除 RF-kill | `sudo rfkill unblock wifi` | ✅ Soft blocked: no |
| 修改配置 | `sudo sed -i 's/managed=false/managed=true/' /etc/NetworkManager/NetworkManager.conf` | ✅ |
| 重启 NetworkManager | `sudo systemctl restart NetworkManager` | ✅ |
| 启用 WiFi | `sudo ip link set wlan0 up` | ✅ state UP |
| 连接 WiFi | `nmcli device wifi connect 'QSTDC' password 'qskj123456' ifname wlan0` | ✅ connected |

**连接成功后的网络信息：**

| 项目 | 值 |
|------|-----|
| 网络名称 | QSTDC |
| 接口 | wlan0 |
| IP 地址 | 192.168.1.201 |
| 信号强度 | 100% |

---

## 预防措施

1. **避免 RF-kill 锁定**：确保系统启动时 WiFi 未被禁用
2. **配置持久化**：确保 NetworkManager 的 `managed=true` 配置持久生效
3. **开机自动连接**：可以使用 `nmcli connection modify` 设置自动连接

```bash
# 设置 WiFi 开机自动连接
nmcli connection modify 'QSTDC' connection.autoconnect yes
```

---

## 相关命令速查

```bash
# 查看网络设备状态
nmcli device status

# 查看 WiFi 列表
nmcli device wifi list

# 断开 WiFi
nmcli device disconnect wlan0

# 删除 WiFi 配置
nmcli connection delete 'QSTDC'

# 查看 IP 地址
ip addr show wlan0

# 测试网络连通性
ping -I wlan0 8.8.8.8
```
