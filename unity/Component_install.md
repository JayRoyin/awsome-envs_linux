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

![](/unity/img/Commponent_Supported_dependency.png)

#### 安装JDK
如上图，我们需要下载JDK11，进入[官网下载](https://www.oracle.com/java/technologies/downloads/#java11)，这里选择的是`jdk-11.0.29_linux-x64_bin.tar.gz`进行下载。

![](/unity/img/Commponent_JDK.png)

下载后直接解压到相关文件夹。

#### 安装SDK
安装SDK之前，我们需要Android 应用开发的官方 IDE，所以需要[Android Studio下载](https://developer.android.google.cn/studio?hl=zh-cn)并安装，下载的页面会自动识别自己的系统类型，所以可以直接点击下载。

下载解压完成之后，需要进入到`android-studio/bin`下找到`studio`,打开安装程序，安装过程也非常简单，所以这里就略过了，之后启动也是通过这个应用程序进行启动。

![](/unity/img/Commponent_Studio_1.png)

打开程序后，进入设置，找到**Android**得到如上界面，然后根据编译器版本所需安装适合自己的**SDK**和**Tools**。

>[\!NOTE]
>可以根据自己的需要，修改和合适的安装路径，建议将Android的相关SDK、NDK都放在同一根目录下，方便管理。

其中我们要注意的是NDK不通过Studio进行安装，我们只需要安装SDK，例如这里使用的是2022.3.62f3，然后选择当前JDK适合的“Android SDK Command-line Tools”版本，我的jdk版本是jdk-11.0.22，所以工具我选了8.0。

![](/unity/img/Commponent_Studio_2.png)

![](/unity/img/Commponent_Studio_3.png)

>[\!NOTE]
>如果页面显示的东西很少，那么切换到**SDK Tools**面版，先勾选“Show Package Details”，显示出更多信息。

完成上面操作后，记得Apply，然后进行下载和安装（需要足够的空间）。

#### 安装NDK
由于NDK的安装的特殊性，我提供了一个[开源地址](https://github.com/android/ndk)，可以查看NDK的项目页面。

去[下载页面](https://github.com/android/ndk/wiki/Unsupported-Downloads)找到适合自己的NDK版本，例如我们需要`r23b`，但是提供的页面没有下载，可以直接点击[下载](https://dl.google.com/android/repository/android-ndk-r23b-linux.zip)获取。





#### 安装gradle

可以去[官网下载](https://services.gradle.org/distributions/)页面查找对应的版本，比如这里我选择的是`gradle-7.6-bin.zip`，记得解压相关的文件夹内。

![](/unity/img/Commponent_Gradle.png)

也可以通过下面的指令进行下载：
```bash
wget https://services.gradle.org/distributions/gradle-7.6-bin.zip
unzip gradle-7.6-bin.zip
```

### Unity 内安卓配置

在完成上面组件的安装后，我的组件所在的结构如下：
```bash
.
├── android-ndk-r23b
├── android-ndk-r23c
├── gradle-7.2
├── gradle-7.6
└── Sdk
```

>**注意：**
>由于我的JDK解压到了编辑器文件夹内，所以我的路径设置也有所不同

在Unity里进行配置，配置如下图：

![](/unity/img/Commponent_Android.png)

至此Android的打包环境已经配置完成

