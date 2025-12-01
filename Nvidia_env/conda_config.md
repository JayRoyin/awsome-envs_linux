conda 安装以及环境配置
===
>更多conda操作指南 [**用户指南**](https://docs.conda.org.cn/projects/conda/en/stable/user-guide/index.html "最好的用户指南")

- [conda 安装以及环境配置](#conda-安装以及环境配置)
  - [conda 环境](#conda-环境)
    - [1. 确保 Conda 已安装](#1-确保-conda-已安装)
    - [2. 添加 Conda 到系统路径](#2-添加-conda-到系统路径)
    - [3. 重启终端并测试 Conda](#3-重启终端并测试-conda)
  - [cuda使用教程](#cuda使用教程)
    - [简单创建环境](#简单创建环境)
    - [使用命令创建特定的环境](#使用命令创建特定的环境)
    - [修改默认配置](#修改默认配置)
  - [FQA](#fqa)
    - [1. 激活conda环境报错](#1-激活conda环境报错)
    - [2. libstdc++.so.6版本升级与链接修复](#2-libstdcso6版本升级与链接修复)

## conda 环境
### 1. 确保 Conda 已安装

首先，检查 Conda 是否已经安装。通常，它的默认安装位置是在 `~/miniconda3` 或 `~/anaconda3` 目录中。你可以使用以下命令查看这些路径：
```bash
ls ~/miniconda3 
ls ~/anaconda3 
```
如果看到 Conda 文件夹存在，继续下面的步骤。如果没有找到，你可能需要重新安装 Miniconda 或 Anaconda。
### 2. 添加 Conda 到系统路径

如果 Conda 已安装但系统找不到它，你可以手动将 Conda 添加到系统路径中。假设 Conda 安装在 `~/miniconda3` 或 `~/anaconda3`，可以将以下内容添加到你的 `~/.bashrc` 文件中：
```bash
 export PATH="$HOME/miniconda3/bin:$PATH"

## 或者

export PATH="$HOME/anaconda3/bin:$PATH"

## 然后刷新环境变量：

source ~/.bashrc 
```
### 3. 重启终端并测试 Conda

重启终端或重新加载 .bashrc 文件后，尝试运行以下命令来测试 Conda：
```bash
conda --version 
## 如果一切正常，这条命令应该会输出 Conda 的版本号。
```

***

## cuda使用教程
### 简单创建环境

Conda 允许您创建独立的环境，每个环境都包含自己的文件、包和包依赖项。 每个环境的内容互不干扰。

创建新环境最基本的方法是使用以下命令
```bash
conda create -n <env-name>
```
要在创建环境时添加包，请在环境名称后指定它们
```bash
conda create -n myenvironment python numpy pandas
```
列出环境，查看所有环境的列表
```bash
conda info --envs
```
将显示环境列表，类似于以下内容
```bash
conda environments:

   base           /home/username/Anaconda3
   myenvironment   * /home/username/Anaconda3/envs/myenvironment
```
>提示
>
>活动环境是用星号 `(*)` 标记的环境。

要将当前环境更改回默认 `base`
```bash
## 进入<env-name>环境
conda activate <env-name>

conda activate
```
>提示
>
>当环境被停用时，其名称不再显示在提示符中，星号 `(*)` 返回到 `base`。 要验证，您可以重复 `conda info --envs` 命令。


***

### 使用命令创建特定的环境

```bash
conda create --name <my-env>
# 这将在 /envs/ 中创建 myenv 环境。此环境中不会安装任何包。
```

要创建具有特定 Python 版本的环境
```bash
conda create -n myenv python=3.9
```
要创建具有特定包的环境
```bash
conda create -n myenv scipy
```
或
```bash
conda create -n myenv python
conda install -n myenv scipy
```
要创建具有特定版本包的环境
```bash
conda create -n myenv scipy=0.17.3
```
或
```bash
conda create -n myenv python
conda install -n myenv scipy=0.17.3
```
要创建具有特定 Python 版本和多个包的环境
```bash
conda create -n myenv python=3.9 scipy=0.17.3 astroid babel
```
### 修改默认配置
```bash
conda config --set auto_activate_base false	# 默认不进入base环境
conda config --set auto_activate_base true	# 默认进入base环境
```

## FQA
### 1. 激活conda环境报错
在sh脚本中，使用`conda activate <env>`时出错：  
`CondaError: Run ‘conda init’ before ‘conda activate’`

在conda activate前加入：
```bash
source ~/anaconda3/etc/profile.d/conda.sh
## 或者添加到bashrc文件中
```
### 2. libstdc++.so.6版本升级与链接修复
在使用conda环境的时候可能会出现系统原有的 ``libstdc++.so.6 版本（6.0.28）``不支持所需的` GLIBCXX `符号，导致某些应用程序无法正常运行。需要升级到更高版本（6.0.34）以支持更多的 C++ 标准库特性。
具体修复请查看[libstdc++.so.6版本升级](libstdc.md)文档

