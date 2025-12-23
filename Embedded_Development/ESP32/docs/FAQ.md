常见问题故障排除 (FAQ)
===
## 1\. 串口连接以及权限

| 错误信息 | 解决方案 |
| :--- | :--- |
| **串口读取报错: `[Errno 13] Permission denied:` 或 `Hint: Try to add user into dialout or uucp group.`** | **a.** 获取串口访问权限,将您的用户添加到 **dialout** 组中（具体方法查看[获取串口访问权限](/Project/ESP32/docs/port_permission.md)）。<br> **b.** 查看摄像头是否接好或则供电正常  |
