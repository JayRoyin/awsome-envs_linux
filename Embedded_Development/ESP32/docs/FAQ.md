常见问题故障排除 (FAQ)
===
## 1\. 串口连接以及权限

| 错误信息 | 解决方案 |
| :--- | :--- |
| **串口读取报错: `[Errno 13] Permission denied:` 或 `Hint: Try to add user into dialout or uucp group.`** | **a.** 获取串口访问权限,将您的用户添加到 **dialout** 组中（具体方法查看[获取串口访问权限](/Project/ESP32/docs/port_permission.md)）。<br> **b.** 查看摄像头是否接好或则供电正常  |


## 

| 错误信息 | 解决方案 |
| :--- | :--- |
| **报错 `No serial data received` 意味着电脑虽然找到了串口（/dev/ttyACM0），但是无法让芯片进入“下载模式”。** |  这是因为 ESP32-S3 使用原生 USB (USB-OTG/JTAG) 作为串口时，自动复位电路有时会失效，或者之前的固件导致 USB 状态异常。<br>  请尝试手动进入下载模式，步骤如下：<br> **1.** 按住板子上的 BOOT 按钮（通常标有 BOOT 或 0）不放。<br> **2.** 按一下然后松开 RESET 按钮（通常标有 RST 或 EN）。<br> **3.** 松开 BOOT 按钮。<br>此时，再次运行烧录命令|

