# 《C++ Primer》系列视频之环境配置

# 学习大纲

* **C++开发有哪些环境需要配置？**
* **如何配置C++开发环境？**

## C++开发有哪些环境需要配置？

首先，我们需要选择一个**操作系统**。`Window`，`Linux`和`MacOs`都可以。

其次，我们需要一个**编译器**，目前可选的有`msvc`、`gcc`和`llvm`。

最后，我们需要选择一款顺手的**IDE**，可选择的有`Visual Studio`、`vscode`、`vim`等等。

如果你是萌新，我的建议是`Windows+Visual Studio`，在安装`vs`的时候会自动安装`msvc`编译器，装完即用，无需任何其他的配置。写完代码F5一键调试/运行，同时还支持图形界面的断点调试，极大方便了新手学习`C++`开发的过程。事实上，我本人最开始也是使用这一套工具学习`C++`的。

如果你熟悉`Linux`系统，我推荐你使用`Linux+vscode+gcc/g++`，只有了解代码编译的流程，才能更好的解决各种编译问题。而`Linux`环境需要你手动编译代码和执行编译结果。

如果你是`vim`大神，那请自便。

本系列视频我准备在前7章基础课程中使用Windows+Visual Studio环境，后续的章节使用`Linux+vscode+gcc/g++`环境，让大家熟悉一下`Linux`下`C++`的开发流程。

## 如何配置C++开发环境？

首先我们来配置`Windows`下的开发环境，我是用的是`Windows 11`操作系统，`IDE`选用`Visual Studio 2022 Community`，个人开发者选用`Community`版本是免费的，而且对于我们来说`Community`版本的功能已经完全够用。

https://visualstudio.microsoft.com/zh-hans/vs/

![vs下载地址](https://s2.loli.net/2022/05/29/EqoMKCfSuxa41Xh.png)

下载完成后双击，

![Installer](https://s2.loli.net/2022/05/29/9XMpqCu6UbYFEvB.png)

点击继续，

![选择安装项](https://s2.loli.net/2022/05/29/ecUJj9QGIdyrT2O.png)

选择”使用C++的桌面开发“，点击安装，Visual Studio Installer开始下载相关组件并且安装到本地。安装完成后首次启动需要你登录，也可以创建一个免费的Microsoft账号登录。

![登录](https://s2.loli.net/2022/05/29/GleoWTSDhimYIBH.png)