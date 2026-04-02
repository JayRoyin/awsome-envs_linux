配置共享文件夹
===

以用 Samba（跨平台通用，Windows/macOS/Linux 都能访问）

# Samba（推荐，全平台兼容）
## 1. 安装 Samba（Ubuntu/Debian）
```bash
sudo apt update
sudo apt install samba -y
```
## 2. 准备共享目录
```bash
# 新建共享目录（也可以直接用你现有路径）
sudo mkdir -p /home/user/shared
```
```bash
# 1. 确保目录归属正确（所有者为 user 用户）
sudo chown -R user:user /home/user/shared
# 2. 开放目录读写权限（兼容所有访问者）
sudo chmod -R 777 /home/user/shared
# 3. 给父目录加执行权限（Samba 必须的穿透权限）
sudo chmod +x /home/user
```


## 3. 配置 Samba
```bash
sudo nano /etc/samba/smb.conf
```

```ini
[user_share]
comment = Shared Folder for user
path = /home/user/shared
browsable = yes
writable = yes
guest ok = no
read only = no
force user = user
force group = user
create mask = 0777
directory mask = 0777
```

> [!NOTE]
> - **force user/group：** 强制所有访问者以 user 身份操作，彻底解决权限不一致
> - **create mask/directory mask：** 新建文件 / 文件夹自动开放 777 权限，避免后续权限问题
> - **guest ok = no：** 关闭匿名访问，用账号密码登录，兼容性 100%

按 Ctrl+O 保存，Ctrl+X 退出。

> [!TIP]  
> 1. `smb.conf` 不能有中文
> 2. 测试配置是否正确 
>   ```bash 
>   testparm
>   ```
>   只要没有报 Error，就是正常的。

## 4. 重启服务 + 放行防火墙
```bash
# 1. 重启 Samba 服务
sudo systemctl restart smbd nmbd
# 2. 设置开机自启
sudo systemctl enable smbd nmbd
# 3. 放行防火墙
sudo ufw allow samba
sudo ufw reload
```
给 **user** 用户设置 Samba 独立密码（Windows/Linux 登录用）
```bash
sudo smbpasswd -a user
```
- 输入两次密码（可以和系统密码一致，也可以单独设）
- 后续 Windows/Linux 访问时，用户名填 user，密码填这里设置的

## 5. 查看本机 IP（其他电脑要用到）
```bash
hostname -I
# 记下形如 192.168.x.x 的地址
```

## 6. 其他电脑访问

**Windows：** 文件管理器地址栏输入
```plaintext
\\192.168.1.105\user_share
```
**macOS：** Finder → 前往 → 连接服务器
```plaintext
smb://192.168.1.105/user_share
```
**Linux：** 文件管理器 → 其他位置 → 连接服务器
```plaintext
smb://192.168.1.105/user_share
```

## 7.SMB永久挂载

### Ubuntu 设置方法

> [!IMPORTANT]
> 在链嗯外一台Linux电脑上安装客户端工具
> **Debian/Ubuntu:**
> ```bash
> sudo apt update && sudo apt install smbclient cifs-utils -y
> ```
> **RHEL/CentOS/Rocky:**
> ```bash
> sudo dnf install samba-client cifs-utils -y
> ```

先创建一个本地挂载目录（**另外一台Linux电脑**）
```bash
sudo mkdir -p /mnt/user_share
```

创建一个存密码的文件（更安全，不暴露密码）
```bash
sudo nano /etc/smbcredentials
```

把下面内容粘贴进去（**把密码改成你自己的**）：
```
username=user
password=你的共享密码
```
按 **Ctrl+O → 回车 → Ctrl+X** 保存退出。

然后设置权限：
```bash
sudo chmod 600 /etc/smbcredentials
```

打开开机自动挂载配置文件
```bash
sudo nano /etc/fstab
```

在**文件最后一行**添加：
```
//192.168.2.***/user_share  /mnt/user_share  cifs  credentials=/etc/smbcredentials,vers=3.0,iocharset=utf8,file_mode=0777,dir_mode=0777  0  0
```

> [!NOTE]
> **列出目标主机所有共享**
> ```bash
> # 基本列出（匿名/访客）
> smbclient -L //192.168.1.100
> 
> # 指定用户名（常用）
> smbclient -L //192.168.1.100 -U your_username
> 
> # 直接带密码（避免交互）
> smbclient -L //192.168.1.100 -U 'your_username%your_password'
> ```
> **交互式进入共享（浏览 / 上传 / 下载）**
> ```bash
> smbclient //192.168.1.100/ShareName -U your_username
> ```
> 进入后常用命令：
> 
>     ls：列出文件
>     cd 目录：进入目录
>     get 远程文件：下载到本地
>     put 本地文件：上传到共享
>     exit：退出


测试挂载（不报错就是成功）
```bash
sudo mount -a
```
查看是否成功
```bash
ls /mnt/user_share
```

> [!TIP]
> - 路径固定在 `/mnt/user_share`
> - 可以像本地文件夹一样读写
> - 密码安全存在系统里，不会泄露

---

# FAQ
## Q1.Windows出现 `错误代码 0x80004005`
**方法一：** 修改 Windows 组策略（专业版 / 企业版）
按 **Win + R**，输入 `gpedit.msc` 并回车。
左侧依次展开：计算机配置 -> 管理模板 -> 网络 -> Lanman 工作站。
找到并双击 “启用不安全的来宾登录”。
选择 “已启用”，点击确定。
重启电脑使配置生效。

**方法二：** 修改注册表（家庭版 / 通用）
按 **Win + R**，输入 `regedit` 并回车。
定位到以下路径：
```plaintext
HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters
```
在右侧空白处右键，新建一个 `DWORD (32 位)` 值，命名为 `AllowInsecureGuestAuth`。
双击它，将数值数据改为 `1`，点击确定。 

**方法三：** 清除 Windows 旧凭据（解决「密码错误 / 权限不足」）
**Windows** 会缓存旧的登录凭据，导致新配置不生效：
打开 **控制面板 → 用户账户 → 凭据管理器 → Windows 凭据**
找到所有包含 `192.168.2.***` 或 `user_share` 的凭据，全部删除
重启电脑，重新访问共享时，手动输入用户名密码：  
- **用户名：** `user`
- **密码：** `smbpasswd` 设置的密码


## Q2.其他 Linux 端：解决「权限不足 / 无法访问」
**1.** 访问方式（用账号密码登录）
**图形化：** 文件管理器 → 其他位置 → 地址栏输入 `smb://192.168.2.***/user_share`
- **用户名：** `user`
- **密码：** `smbpasswd` 设置的密码

命令行挂载（永久访问）：
```bash
    # 1. 安装客户端
    sudo apt install cifs-utils -y
    # 2. 创建挂载目录
    sudo mkdir -p /mnt/user_share
    # 3. 挂载共享（替换密码为你的smbpasswd密码）
    sudo mount -t cifs //192.168.2.***/user_share /mnt/user_share -o username=user,password=你的密码,uid=$(id -u),gid=$(id -g)
    # 4. 访问目录
    ls /mnt/user_share
```

**2.** 若仍权限不足，排查 Linux 客户端
```bash
# 1. 测试连通性
ping 192.168.2.***
# 2. 测试Samba连接
smbclient //192.168.2.***/user_share -U user
# 输入密码，若进入smb: \> 提示符，说明配置正常
```