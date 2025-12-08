在 Ubuntu 上安装 Docker Engine
===
- [在 Ubuntu 上安装 Docker Engine](#在-ubuntu-上安装-docker-engine)
  - [操作系统要求](#操作系统要求)
  - [卸载旧版本](#卸载旧版本)
  - [使用 `apt` 软件源进行安装](#使用-apt-软件源进行安装)
    - [1. 设置 Docker 的 apt 软件仓库。](#1-设置-docker-的-apt-软件仓库)
    - [2. 安装 Docker 软件包。](#2-安装-docker-软件包)
    - [3.运行 `hello-world` 镜像来验证安装是否成功：](#3运行-hello-world-镜像来验证安装是否成功)
  - [卸载 Docker Engine](#卸载-docker-engine)
  - [**返回主目录**](#返回主目录)

## 操作系统要求
要安装 Docker Engine，您需要以下 Ubuntu 版本之一的 64 位版本：
  -  Ubuntu Questing 25.10
  -  Ubuntu Plucky 25.04
  -  Ubuntu Noble 24.04 (LTS)
  -  Ubuntu Jammy 22.04 (LTS)

适用于 Ubuntu 的 Docker Engine 与 x86_64（或 amd64）、armhf、arm64、s390x 和 ppc64le（ppc64el）架构兼容。

> [!NOTE]
> 虽然可能可以安装在 Linux Mint 等 Ubuntu 衍生发行版上，但官方并不支持。

## 卸载旧版本

在安装 Docker Engine 之前，需要卸载任何冲突的软件包。

您的 Linux 发行版可能提供非官方的 Docker 软件包，这些软件包可能与 Docker 官方提供的软件包冲突。您必须先卸载这些非官方软件包，然后才能安装官方版本的 Docker Engine。

需要卸载的非官方软件包有：

`docker.io`
`docker-compose`
`docker-compose-v2`
`docker-doc`
`podman-docker`

此外，`Docker Engine` 依赖于 `containerd` 和 `runc Engine` 将这些依赖项打包成一个 bundle： `containerd.io` 。如果您之前安装过 `containerd` 或 `runc` ，请将其卸载，以避免与 `Docker Engine` 自带的版本冲突。

运行以下命令卸载所有冲突的软件包：
```bash
 sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```
`apt` 可能会报告您未安装任何这些软件包。

存储在 `/var/lib/docker/` 中的镜像、容器、卷和网络并非 卸载 Docker 时会自动删除。如果您想从头开始…… 全新安装，并最好清除所有现有数据，请阅读以下内容，<a href="#lable">**卸载 Docker Engine**</a>部分。



## 使用 `apt` 软件源进行安装

在新主机上首次安装 Docker Engine 之前，需要先设置 Docker apt 软件仓库。之后，就可以从该仓库安装和更新 Docker 了。

### 1. 设置 Docker 的 apt 软件仓库。

```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

### 2. 安装 Docker 软件包。

要安装最新版本，请运行：
```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

要安装特定版本的 Docker Engine，首先列出存储库中可用的版本：
```bash
apt list --all-versions docker-ce

docker-ce/noble 5:29.0.4-1~ubuntu.24.04~noble <arch>
docker-ce/noble 5:29.0.3-1~ubuntu.24.04~noble <arch>
...
```
选择所需版本并安装：
```bash
 VERSION_STRING=5:29.0.4-1~ubuntu.24.04~noble

 sudo apt install docker-ce=$VERSION_STRING docker-ce-cli=$VERSION_STRING containerd.io docker-buildx-plugin docker-compose-plugin
```
>[!NOTE]
>安装完成后，Docker 服务会自动启动。要验证 Docker 是否正在运行，请使用：
>```bash
>sudo systemctl status docker
>```
>某些系统可能已禁用此功能，需要手动启动：
>```bash
>sudo systemctl start docker
>```

### 3.运行 `hello-world` 镜像来验证安装是否成功：
```bash
sudo docker run hello-world
```
此命令下载测试镜像并在容器中运行。容器运行后，会打印确认信息并退出。
如果出现`Unable to find image ‘hello-world:latest’ locally`，请转到[镜像拉取超时问题解决方法](timeout.md)
您已成功安装并启动 Docker Engine。



<span id="lable"></span>
## 卸载 Docker Engine

卸载 `Docker Engine`、`CLI`、`containerd` 和 `Docker Compose` 软件包：
```bash
 sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```
主机上的镜像、容器、卷或自定义配置文件不会自动删除。要删除所有镜像、容器和卷：
```bash
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```
删除源列表和密钥环
```bash
sudo rm /etc/apt/sources.list.d/docker.sources
sudo rm /etc/apt/keyrings/docker.asc
```
您必须手动删除所有已编辑的配置文件。

---
[**返回主目录**](/README.md)
---