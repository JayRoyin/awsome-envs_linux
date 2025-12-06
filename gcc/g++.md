环境GCC/G++
===
## GCC版本切换
使用update-alternatives 进行版本切换
`
sudo update-alternatives --config gcc
`
示例如下
```bash
有 4 个候选项可用于替换 gcc (提供 /usr/bin/gcc)。

  选择       路径           优先级  状态
------------------------------------------------------------
  0            /usr/bin/gcc-14   14        自动模式
* 1            /usr/bin/gcc-10   10        手动模式
  2            /usr/bin/gcc-11   11        手动模式
  3            /usr/bin/gcc-14   14        手动模式
  4            /usr/bin/gcc-9    9         手动模式

要维持当前值[*]请按<回车键>，或者键入选择的编号：2
update-alternatives: 使用 /usr/bin/gcc-11 来在手动模式中提供 /usr/bin/gcc (gcc)
```

## 注册到环境
```bash
## 批量添加
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 20 --slave /usr/bin/g++ g++ /usr/bin/g++-11

## 正常添加
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 20

## 移除GCC
sudo update-alternatives --remove gcc /usr/bin/gcc-9
```

``--install`` 代表我们需要注册一个新的服务名

``/usr/bin/gcc`` 代表我们目标的最终地址，切换软链接的时候会切换该地址的软链接

``gcc`` 代表我们用于管理服务的名字

``/usr/bin/gcc-11`` 代表被管理的命令的绝对路径（会用这个命令来替换第二个参数的软链接）

``20`` 代表优先级，数字越大优先级越高。

``--slave`` 代表从属命令，参数顺序和前面这几个是一样的，配置的是g++命令
 
##查看当前版本
```bash
## 显示版本
gcc --version
g++ --version
gcov --version

## 使用以下命令列出系统中所有与 GCC 相关的可执行文件：
ls /usr/bin/gcc*
ls /usr/bin/g++*
```
***

