组件安装（Linux版）
===

***
由于我们编辑器的安装不是通过Unity Hub安装的，所以我们不能直接通过Hub便捷安装组件，所以我们需要手动进行安装。
这里以`Unity 2022.3.62f3`为例，首先进入[Unity 2022.3.62f3 Released](https://unity.com/releases/editor/whats-new/2022.3.62f3)找到自己要安装的组件。如下图所示：

![](/unity/img/Commponent_1.png)

## Windows

### 下载组件包
点击 **Windows Build Support**下载，我们会得到`UnitySetup-Windows-Mono-Support-for-Editor-2022.3.62f3.pkg`这个文件，虽然我们得到的是pkg后缀的文件，但是我还是可以解压。

### 解压安装
我们使用pkg相关的指令进行解压，方法如下：
首先解压 .pkg 包本身
```bash
7z x "UnitySetup-Windows-Mono-Support-for-Editor-2022.3.62f3.pkg" -o"Temp" && cd Temp
```
解压内部的 Payload 文件以获取真实文件
```bash
7z x "Payload~" -o"WindowsStandaloneSupport"
```
>**注意：**
>内部通常包含一个名为 Payload 或 Payload~ 的无后缀文件，实际文件都在这里面。

解压完成后，我们需要将这个包放进Unity编辑器的文件夹内，例如我们的文件路经为：
`Your Editor Patch /2022.3.62f3/Editor/Data/PlaybackEngines`

```bash
mv WindowsStandaloneSupport /YourPatch/Editor/Data/PlaybackEngines
```

## IOS 

点击 **iOS Build Support**下载，我们会得到`UnitySetup-iOS-Support-for-Editor-2022.3.62f3.tar.xz`这个文件，我们直接解压就行，解压为`iOSSupport`的文件夹后放到`Your Editor Patch /2022.3.62f3/Editor/Data/PlaybackEngines`的目录下

## Android
### 下载组件包
点击 **Android Build Support**下载，我们会得到`UnitySetup-Android-Support-for-Editor-2022.3.62f3.pkg`这个文件，虽然我们得到的是pkg后缀的文件，但是我还是可以解压。

### 解压安装
我们使用pkg相关的指令进行解压，方法如下：
首先解压 .pkg 包本身
```bash
7z x "UnitySetup-Android-Support-for-Editor-2022.3.62f3.pkg" -o"Temp" && cd Temp
```
解压内部的 Payload 文件以获取真实文件
```bash
7z x "Payload~" -o"AndroidPlayer"
```
>**注意：**
>内部通常包含一个名为 Payload 或 Payload~ 的无后缀文件，实际文件都在这里面。

### 环境配置
要为 Android 构建和运行应用程序，必须安装 Unity Android Build Support 平台模块。还需要安装 Android 软件开发工具包（SDK）和原生开发工具包（NDK）才能在 Android 设备上构建和运行代码。默认情况下，Unity 会安装基于 OpenJDK 的 Java 开发工具包。

具体环境要求可以查看[官方文档](https://docs.unity3d.com/2022.3/Documentation/Manual/android-supported-dependency-versions.html)



#### 安装SDK