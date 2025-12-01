libstdc++.so.6 版本升级与链接修复技术文档
===
- [libstdc++.so.6 版本升级与链接修复技术文档](#libstdcso6-版本升级与链接修复技术文档)
  - [问题描述](#问题描述)
  - [环境信息](#环境信息)
  - [修复步骤](#修复步骤)
  - [验证结果](#验证结果)
  - [故障排除](#故障排除)
    - [备份回退](#备份回退)
    - [手动修复链接](#手动修复链接)
    - [Illegal instruction错误](#illegal-instruction错误)


## 问题描述

在conda的环境下,系统原有的 `libstdc++.so.6` 版本（6.0.28）不支持所需的 GLIBCXX 符号，导致某些应用程序无法正常运行。需要升级到更高版本（6.0.34）以支持更多的 C++ 标准库特性。

下面均以**aarch64**架构为例

## 环境信息

- **系统架构**: ARM aarch64/X86_64
- **系统版本**: Ubuntu
- **原版本**: libstdc++.so.6.0.28
- **目标版本**: libstdc++.so.6.0.34
- **版本来源**: Conda 环境 (`/home/unitree/anaconda3/envs/botaction_env/lib/`)

## 修复步骤

1. 验证版本兼容性

```bash
# 查看自己系统架构
uname -all
```
建议先退出conda环境
```bash
conda activate 
```
首先检查两个版本的 GLIBCXX 支持情况：
```bash
# 检查原系统版本支持的GLIBCXX
strings /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.28 | grep GLIBCXX

# 检查conda环境版本支持的GLIBCXX
strings /home/unitree/anaconda3/envs/botaction_env/lib/libstdc++.so.6.0.34 | grep GLIBCXX
```
输出效果如下
```bash
GLIBCXX_3.4
GLIBCXX_3.4.1
GLIBCXX_3.4.2
GLIBCXX_3.4.3
GLIBCXX_3.4.4
...
GLIBCXX_3.4.27
GLIBCXX_3.4.28
GLIBCXX_3.4.29
GLIBCXX_3.4.30
GLIBCXX_DEBUG_MESSAGE_LENGTH
```
确认 conda 环境的 6.0.34 版本支持更多的 GLIBCXX 符号（包括 GLIBCXX_3.4.29 到 GLIBCXX_3.4.34）。
>注意
>如果在x86_64架构下,则需要将`/usr/lib/aarch64-linux-gnu/libstdc++.so`中的`aarch64-linux-gnu`换成`x86_64-linux-gnu`
2. 备份原版本（可选但推荐）

```bash
sudo cp /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.28 /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.28.backup
```

3. 移除旧的符号链接

```bash
sudo rm /usr/lib/aarch64-linux-gnu/libstdc++.so.6
```

4. 复制新版本库文件

```bash
sudo cp /home/unitree/anaconda3/envs/botaction_env/lib/libstdc++.so.6.0.34 /usr/lib/aarch64-linux-gnu/
```

5. 创建新的符号链接

```bash
sudo ln -sf /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.34 /usr/lib/aarch64-linux-gnu/libstdc++.so.6
```

6. 更新动态链接器缓存

```bash
sudo ldconfig
```

7. 验证安装

```bash
# 检查符号链接
ls -l /usr/lib/aarch64-linux-gnu/libstdc++*

# 验证GLIBCXX支持
strings /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.34 | grep GLIBCXX
```

## 验证结果

修复完成后，系统应显示：

```bash
lrwxrwxrwx 1 root root       46 Nov 18 10:26 /usr/lib/aarch64-linux-gnu/libstdc++.so.6 -> /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.34
-rw-r--r-- 1 root root  1907976 Jul  9  2023 /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.28
-rwxr-xr-x 1 root root 14384592 Nov 18 10:26 /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.34
```

且新版本支持 GLIBCXX_3.4.29 到 GLIBCXX_3.4.34 等更多符号。

然后进入所需的conda环境中
```bash
conda activate <env-name>
```

尝试`cmake`  `make`之类的命令进行编译,然后查看`libstdc`是否可用,如果不能使用请<a href="#lable">**手动修复**</a>

***

## 故障排除
### 备份回退
如果遇到问题，可以回滚到原版本：

```bash
sudo rm /usr/lib/aarch64-linux-gnu/libstdc++.so.6
sudo ln -sf /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.28 /usr/lib/aarch64-linux-gnu/libstdc++.so.6
sudo ldconfig
```

### 手动修复链接

如果出现下列问题<span id="lable"></span>
```bash
XXX:error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory
```
请执行下列指令
```bash
# 从conda环境复制到系统目录（临时解决）
sudo cp /home/unitree/anaconda3/envs/botaction_env/lib/libstdc++.so.6.0.34 /usr/lib/aarch64-linux-gnu/

# 创建正确的符号链接
sudo ln -sf /usr/lib/aarch64-linux-gnu/libstdc++.so.6.0.34 /usr/lib/aarch64-linux-gnu/libstdc++.so.6

# 更新库缓存
sudo ldconfig
```
### Illegal instruction错误

```bash
sudo /home/unitree/anaconda3/envs/botaction_env/lib/libstdc++.so.6.0.34 /usr/lib/aarch64-linux-gnu/
Illegal instruction
```
`Illegal instruction` 错误通常发生在以下情况：

- 1. 编译时使用的CPU架构与运行时的CPU架构不匹配

- 2. 库文件版本不兼容

- 3. 尝试直接执行共享库文件（.so文件）