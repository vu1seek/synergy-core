---
created: 2024-10-05T00:49:46 (UTC -07:00)
tags: [库文件,简单记,SERVICE,macOS,QT5,非技术,中科大,XCODE,X86,DMG,WINDOWS,Release,GIT,SHELL,官方教程,msi,ADD,OPTIONAL,MBP,官方版,MAINTENANCE,COMPILER,ROSETTA,辅助功能,App store,命令行,BUILD,最新版本,二进制,国际化,背景图,GITHUB,NAME,最后一个,HTTPS,BUG,下载地址,解决方法,无所谓,记不清,可以使,不需要,一条命,没有了,找不到,编辑器,MAC]
source: https://www.bilibili.com/read/cv19053031/
author: Snnnotname
---

# Synergy编译、去激活、汉化：macOS+Windows保姆级教程 - 哔哩哔哩

> ## Excerpt
> 补充（2022-12-04）：源码和安装包放到github上了https://github.com/FLZeng/synergy-core/releases最近添了一台M1芯片的MBP，之前用的键鼠共享软件Mouse without Borders没有macOS版本，于是又想起来开源的Synergy，可以编译一个macOS版本，顺便也编译了新版的Windows版本，并且修了源码中的一些bug。编译好的文件放到蓝奏云了，非技术党用户不用看后面的编译教程，下载就能用：地址：https://wwp.l

---
补充（2022-12-04）：

源码和安装包放到github上了

https://github.com/FLZeng/synergy-core/releases

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4aa545dccf7de8d4a93c2b2b8e3265ac0a26d216.png@progressive.webp)

最近添了一台M1芯片的MBP，之前用的键鼠共享软件Mouse without Borders没有macOS版本，于是又想起来开源的Synergy，可以编译一个macOS版本，顺便也编译了新版的Windows版本，并且修了源码中的一些bug。  

编译好的文件放到蓝奏云了，非技术党用户不用看后面的编译教程，下载就能用：

> 地址：https://wwp.lanzoub.com/b02dremyd
> 
> 密码：5auq

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/14131e5ab80d8b1411022e887c2c0813584ddad4.png@!web-article-pic.avif)

下载地址

包含三个文件：

-   Synergy-1.14.6-macOS-arm64.dmg是macOS的安装包，使用于Apple Silicon（M1系列、M2）；
    
-   Synergy-1.14.6-Win-x64-Installer.zip是Windows 64位的安装包，解压后得到msi文件（因为蓝奏云不支持上传msi格式）；
    
-   Synergy-1.14.6-Win-x64-Portable.zip是Windows 64位便携版，解压免安装，直接运行synergy。
    

macOS版和Windwos便携版运行为桌面程序模式（Desktop）,可以在应用首选项中最小化到托盘；Windows安装版运行为服务模式（Service）,安装后会注册一个Windows服务，首次配置后不再需要启动图形界面，后台服务自动运行。

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4adb9255ada5b97061e610b682b8636764fe50ed.png@progressive.webp)

1\. 获取源码

从Github代码仓库获取，当前最新版本是1.14.6：

```shell
git clone https://github.com/symless/synergy-core.git
```

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4adb9255ada5b97061e610b682b8636764fe50ed.png@progressive.webp)

2\. macOS平台编译Synergy

编译过程主要参考官方文档：https://github.com/symless/synergy-core/wiki/Compiling，但是有一些坑。

## 2.1 安装xcode

直接从App Store安装xcode，会自动装上开发工具链。安装后要打开一次，选择同意license，然后就不用管了。

## 2.2 安装brew

brew是macOS的包管理工具，后面要用它来安装一些依赖包。直接执行官方脚本会报错，需要先把脚本下载下来，把里面的源改成国内源。

```shell
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh > install.sh
```

修改脚步里的这两个变量，替换为中科大源：

```shell
HOMEBREW_BREW_DEFAULT_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git" HOMEBREW_CORE_DEFAULT_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
```

然后执行脚本安装：

```shell
chmod a+x install.sh sh -C ./install.sh
```

## 2.3 安装cmake

```shell
brew install cmake libsodium
```

## 2.4 安装OpenSSL

```shell
brew install openssl
```

编辑~/.zshrc，添加openssl执行路径：

```shell
export PATH="/opt/homebrew/opt/openssl@3/bin:$PATH"
```

## 2.5 安装Qt 5

## 2.5.1 问题

我先是安装了最新版本的Qt 6，然后发现Synergy是用Qt 5写的，编译提示语法不支持，于是只好换成Qt 5。 最后一个长期支持版是Qt 5.12.12，但社区预编译的Qt是x86架构，而我M1的mac是arm64架构，虽然通过rosetta转译可以运行，但编译时就会遇到问题：

-   无法编译成x86架构的程序：因为依赖的包（OpenSSL）是arm64架构；
    
-   无法编译成ARM64架构的程序：因为要链接的Qt二进制库是x86架构；
    

只有把二者的架构统一才能成功编译。我选择把Qt换成arm64架构的，官方没提供，那就自己编译。

## 2.5.2 编译安装Qt

先装几个依赖：

```shell
brew install pcre2 harfbuzz freetype
```

编译Qt Creator（不是必须）还要装另外几个：

```shell
brew install ninja python brew install --build-from-source llvm
```

之前安装的Qt 5.12.12可以保留，因为我们只是需要arm64版本的Qt库，Qt Creator无所谓，省得自己编译。

然后下载Qt源码，Qt 5的最后一个版本是5.15.6：

```shell
https://download.qt.io/archive/qt/5.15/5.15.6/single/qt-everywhere-opensource-src-5.15.6.tar.xz
```

解压后进入代码目录，建立一个build目录：

```shell
cd qt-everywhere-src-5.15.6 mkdir build cd build
```

配置、编译、安装：

```shell
../configure -release -prefix /usr/local/share/qt-5.15.6 -nomake examples -nomake tests QMAKE_APPLE_DEVICE_ARCHS=arm64 -opensource -confirm-license -skip qt3d -skip qtwebengine make -j8 make install
```

编译好的Qt就被安装到了/usr/local/share/qt-5.15.6目录下。编辑~/.zshrc，添加Qt执行程序路径：

```shell
export PATH="/usr/local/share/qt-5.15.6/bin:$PATH"
```

官方教程：

```shell
https://wiki.qt.io/Building_Qt_5_from_Git
```

## 2.5.3 配置Qt Creator

如果想继续编译Qt Creator，可以参考下面两个文档：

```shell
https://www.reddit.com/r/QtFramework/comments/ll58wg/how_to_build_qt_creator_for_macos_arm64_a_guide/ https://github.com/qt-creator/qt-creator/blob/master/README.md#compiling-qt-creator
```

也可以下载官方编译的x86架构的包：

```shell
https://download.qt.io/official_releases/qtcreator/
```

我是通过online installer安装的。 然后打开Qt Creator，进入 Preferences-Kits-Qt Versions 选项卡，点击添加按钮（或者是Link with Qt，记不清了），地址填 /usr/local/share/qt-5.15.6/bin/qmake ，会自动识别目录下的工具链，然后点OK完成：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/d93ed3650720bdcb9b70f88c2fafb75c214b160e.png@1256w_762h_!web-article-pic.avif)

添加自编译的Qt

进入 Preferences-Kits-CMake 选项卡，点击Add，Name随便填，我是“brew CMake”，Path填 /opt/homebrew/bin/cmake：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/b0424c031dc6b30d6c2fdb60d7800466993519ad.png@1256w_762h_!web-article-pic.avif)

添加cmake

再进入 Preferences-Kits-Kits 选项卡，点击Add，Compiler选 Apple Clang (arm64)，Qt version选刚刚添加的 Qt 5.15.6，Cmake Tool选刚刚添加的brew Cmake，然后在Environment中加上 MAKEFLAGS=-j8，开启多核编译：  

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/5643799dc12670901f6429d81c7e0deddef484b7.png@1256w_762h_!web-article-pic.avif)

配置编译工具链

## 2.5.4 OpenSSL库文件路径的问题

在/etc/profile、~/.bashrc等文件中添加的环境变量，只在shell中生效，启动Qt Creator仍然没有获取到。为了让环境变量在Qt Creator中生效，有两种解决方法：

**方法1：从shell启动Qt Creator**

```shell
export OPENSSL_ROOT_DIR="/opt/homebrew/opt/openssl@3" /usr/local/share/qt-5.12.12/Qt\ Creator.app/Contents/MacOS/Qt\ Creator
```

**方法2：使用launchctl命令添加环境变量**

```shell
launchctl setenv OPENSSL_ROOT_DIR "/opt/homebrew/opt/openssl@3"
```

然后启动Qt Creator就可以获取了。

## 2.6 编译Synergy

在Qt Creator中点击文件-打开文件或项目，打开synergy-core/CMakeLists.txt：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/35b075ab5966d7dcc52f612ad4a7fc281c996b68.png@1256w_720h_!web-article-pic.avif)

导入synergy-core项目

然后在项目配置中选择刚刚配置的Desktop Qt-5.15.6 kit，展开详情，只勾选Release，然后点击Configure Project：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/86d6945e0a5f3401c55888c67a4fd463ef0880ea.png@1256w_762h_!web-article-pic.avif)

配置工具链

会自动执行cmake，等执行完成后，在项目上右键，点击构建，开始编译，编译完成后显示cmake正常退出：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/2be81bba6567320e96f30f158d82f487231418d1.png@1256w_720h_!web-article-pic.avif)

构建

然后在synergy-core同级目录下可以看到build-synergy-core-xxx/目录下，生成了bin/synergy系列可执行文件，以及bundle/Synergy.app：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/a6aa2a0df938b81d56a0b7dea18dcb0b3a93eb47.png@1256w_816h_!web-article-pic.avif)

构建结果

## 2.7 命令行编译Synergy（Optional）

也可以使用命令行编译，这样就不需要Qt Creator了：

```shell
cd synergy-core mkdir build cd build export QT_PATH=/usr/local/share/Qt5.15.6 export PATH=$PATH:/usr/local/bin:$QT_PATH/bin cmake -DOPENSSL_ROOT_DIR=/opt/homebrew/opt/openssl@3 -DOPENSSL_LIBRARIES=/opt/homebrew/opt/openssl@3/lib -DCMAKE_OSX_DEPLOYMENT_TARGET=12.6 -DCMAKE_OSX_ARCHITECTURES=arm64 -DCMAKE_BUILD_TYPE=$CMAKE_BUILD_TYPE .. make
```

这样编译的结果在synergy-core/build目录下。

## 2.8 打包Synergy.app

在Synergy.app上右键，点击显示包内容，然后在Contents目录下创建一个MacOS目录，把bin目录下的二进制文件都复制进去：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/605156d0635b5cedf1e5bef4433c6b9853ce4387.png@1256w_816h_!web-article-pic.avif)

Synergy.app

这个时候Synergy.app就可以运行了，但还存在问题。Synergy.app会申请辅助功能权限，给它授权后，实际上只授权给了主程序synergy，synergyc和synergys运行的时候仍然会提示没有assistive devices权限。要将Synergy.app重新签名一下：

```shell
cd bundle macdeployqt Synergy.app -executable="Synergy.app/Contents/MacOS/synergyc" -executable="Synergy.app/Contents/MacOS/synergys" codesign --sign - --force --deep Synergy.app
```

关于签名覆盖可以看这几篇文章：  

```shell
https://www.idczone.net/news/3888.html/ https://zhuanlan.zhihu.com/p/363120823
```

## 2.9 创建映像文件

将Synergy.app打包进dmg映像文件中，用于发布。主要步骤就是通过系统自带的磁盘工具创建一个空白映像文件，然后把Synerg.app拖进去。如何创建美观的dmg，可以看这篇文章：

```shell
https://blog.pangao.vip/Mac%E6%89%93%E5%8C%85dmg%E6%96%87%E4%BB%B6(%E6%9B%B4%E6%8D%A2%E8%83%8C%E6%99%AF%E5%9B%BE)/
```

Synergy的背景图片在Synergy.app/Contents/Resources目录下：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/0de016a0067dd1298b3007d26cd2270cd632c74f.png@1256w_224h_!web-article-pic.avif)

资源图片

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4adb9255ada5b97061e610b682b8636764fe50ed.png@progressive.webp)

## 3\. Windows平台编译Synergy

Windows平台下的编译过程在我之前的文章[鼠标共享神器Synergy的编译打包教程](https://www.bilibili.com/read/cv11942391?spm_id_from=333.999.0.0)里讲过，这里再简单记录一下。

## 3.1 编译环境配置

配置步骤：

1.  安装git：Git for Windows
    
2.  安装Visual Studio 2019：Visual Studio Community 2019 with Updates
    
3.  安装Windows 10 SDK：Windows 10 SDK
    
4.  安装Bonjour SDK for Windows：Bonjour SDK for Windows v3.0
    
5.  安装Qt 5：Qt 5.14.2
    
6.  安装OpenSSL：OpenSSL
    
7.  安装CMake：CMake
    
8.  安装Wix toolchain（打包用到）：The Wix toolchain
    

## 3.2 编译Synergy

编译命令：

```shell
cd synergy-core mkdir build cd build call "D:\ProgramFiles\Develop\Microsoft Visual Studio\2019\Professional\VC\Auxiliary\Build\vcvarsall.bat" x64 cmake -G "Visual Studio 16 2019" -DCMAKE_BUILD_TYPE=Release .. msbuild synergy-core.sln /p:Platform="x64" /p:Configuration=Release /m
```

前面几条命令只要执行一次，后面每次编译执行最后一条命令就行。编译后会在synergy-core\\build\\bin\\Release目录下生成可执行程序，执行synergy.exe可以测试效果。

## 3.3 打包Synergy.msi

打包命令：

```shell
cd synergy-core\build\installer msbuild /p:Configuration=Release
```

会在synergy-core\\build\\installer\\bin\\Release目录下生成Synergy.msi安装包，用于发布。

## 3.4 OpenSSL库的问题

以前的本版在synergy-core/ext/openssl/windows/x64/bin/目录下是有openssl库文件的，现在整个openssl目录都没有了，打包的时候会提示找不到openssl.exe、libssl-1\_1-x64.dll、libcrypto-1\_1-x64.dll这几个文件。需要从安装OpenSSL-Win64安装目录拷过来，放到synergy-core/build/bin/Release/OpenSSL目录下。 

还有个问题是，我安装的OpenSSL是3.0.5版本，Synergy写死了用1.1版本，要改一下synergy-core/dist/wix/Product.wxs文件，将其中的libssl-1\_1-x64.dll和libcrypto-1\_1-x64.dll改为libssl-3-x64.dll和libcrypto-3-x64.dll：

```shell
<?if $(var.Platform) = x64 ?> <File Source="$(var.OpenSSLBinPath)/libssl-3-x64.dll"/> <File Source="$(var.OpenSSLBinPath)/libcrypto-3-x64.dll"/> <?else ?> <File Source="$(var.OpenSSLBinPath)/libssl-3.dll"/> <File Source="$(var.OpenSSLBinPath)/libcrypto-3.dll"/> <?endif ?>
```

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4adb9255ada5b97061e610b682b8636764fe50ed.png@progressive.webp)

## 4\. 去除激活验证

Synergy验证逻辑涉及下面这些类：

```shell
LicenseManager // 提供激活验证的入口 --> AppConfig // 全局的配置参数，其中包括一个SeriaKey和Edition --> SerialKey // 保存Key的字符串、版本、过期时间等信息 --> SerialKeyType // Key的类型，有trial、subscription、maintenance --> SerialKeyEdition // Key的版本，比如unregistered、basic、lite、pro、ultimate --> SerialKeyParser // 解析plane serial中的版本、过期时间等信息
```

主要修改内容：

-   去除了LicenseManager中的所有验证逻辑和提示消息；
    
-   将SerialKey的过期时间设置为3022年末；
    
-   将SerialKeyType固定为maintenance；
    
-   将SerialKeyEdition固定为ultimate。
    

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4adb9255ada5b97061e610b682b8636764fe50ed.png@progressive.webp)

## **5\. 界面汉化**

官方版本的国际化是残缺版，很久没有与界面改动同步更新了。执行下面的命令，更新国际化文件：

```shell
lupdate synergy-core/src/gui/gui.pro
```

会自动抓取界面和代码中要翻译的文本，更新到synergy-core/src/gui/res/lang/目录下的.ts系列文件中。我们汉化只需要修改其中的中文gui\_zh-CN.ts文件。用Qt Linguist打开可以很直观地编辑每个界面的翻译文本：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/1ac11c45a3b6851bd2467d7dc58cfb231650273f.png@1256w_836h_!web-article-pic.avif)

Qt语言家

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/4adb9255ada5b97061e610b682b8636764fe50ed.png@progressive.webp)

## **6\. 自适应高分屏放缩**

使用Qt Creator的资源编辑器打开synergy-core/src/gui/res/Synergy.qrc，添加/qt/etc前缀，然后在其下添加qt.conf文件：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/f7569639a6ddcc179d39f661805aab69383892db.png@1256w_762h_!web-article-pic.avif)

添加qt.conf

编辑qt.conf文件的内容：

```shell
[Platforms] WindowsArguments = dpiawareness=0
```

最终的运行效果：

![](Synergy%E7%BC%96%E8%AF%91%E3%80%81%E5%8E%BB%E6%BF%80%E6%B4%BB%E3%80%81%E6%B1%89%E5%8C%96%EF%BC%9AmacOS+Windows%E4%BF%9D%E5%A7%86%E7%BA%A7%E6%95%99%E7%A8%8B%20-%20%E5%93%94%E5%93%A9%E5%93%94%E5%93%A9/7ea2e8f3fcf340e264afc5b07047615a98208038.png@1256w_900h_!web-article-pic.avif)

Synergy界面

加上改bug前前后后折腾了一周，大大小小的问题记得的都写了，就写到这里吧。
