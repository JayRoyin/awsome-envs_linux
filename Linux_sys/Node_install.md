# Linux 安装 Node.js

[⬅ 返回总览](../README.md) · [APT 安装](#使用apt进行安装) · [NVM 安装](#使用nvm进行安装推荐) · [版本冲突](#解决版本冲突)

## 使用APT进行安装

安装**Node.js**和**npm**
```bash
sudo apt update
sudo apt install nodejs npm
```
验证安装
```bash
node --version
npm --version
```
> [!NOTE]  
> 一般情况下安装的版本由当前系统的版本所决定的，比如我的系统版本为**ubuntu 22.04**，那么安装的版本为：
> - **node：** v10.19.0
> - **npm：**  6.14.4



## 使用nvm进行安装（推荐）

### 下载NVM
#### 使用官方源
该文档基于[官方NVM文档](https://nvm.uihtm.com/doc/download-nvm.html)进行整理归纳的

安装或更新nvm，您应该运行安装脚本。为此，您可以手动下载并运行脚本，也可以使用以下cURL或Wget命令：
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```
或
```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```
运行上述任一命令都会下载一个脚本并运行它。该脚本将nvm存储库克隆到~/.nvm，并尝试将下面代码段中的源代码行添加到正确的配置文件（~/.bashrc、~/.bash_profile、~/.zshrc或~/.profile）中。如果发现安装脚本正在更新错误的配置文件，请将$profile env var设置为配置文件的路径，然后重新运行安装脚本。
```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

#### 使用Gitee镜像源安装
```bash
git clone https://gitee.com/mirrors/nvm.git ~/.nvm
```
进入目录并切换到最新版本(或者指定版本)
```bash
cd ~/.nvm
git checkout v0.40.3
```
配置环境变量
```bash
echo 'export NVM_DIR="$HOME/.nvm"' >> ~/.bashrc
echo '[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"' >> ~/.bashrc

# 重新加载配置
source ~/.bashrc
```

验证 NVM 是否安装成功
```bash
nvm --version
```

使用NVM快速升级，我们以Node 20为例安装LTS版本：
```bash
nvm install 20 --lts
nvm alias default 20
```

验证安装
```bash
node --version
npm --version
```


> [!NOTE]  
> 查看NVM安装的所有版本
> ```bash
> ls ~/.nvm/versions/node/
> ```
查看安装位置：
```bash
which node
which npm
```

### 解决版本冲突

如果安装后仍是旧版本，可能因为PATH优先级问题：
```bash

# 查看所有node位置
which -a node

# 查看当前PATH
echo $PATH

# 删除旧版本（Ubuntu/Debian）
sudo apt remove nodejs npm
sudo apt autoremove

# 或者手动删除
sudo rm /usr/bin/node
sudo rm /usr/bin/npm
```
