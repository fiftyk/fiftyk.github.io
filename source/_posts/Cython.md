---
layout: os
title: OS Mountain Lion 10.8.2 安装 Cython
date: 2012-12-27
categories:
- iteye
tags:
- python
---

[原文](http://fiftyk.iteye.com/admin/blogs/1754733)

使用 Python 开发的程序，运行速度上不太满意，碰巧看到赖勇浩 CSDN 博客中翻译的 [Cython 三分钟入门](http://blog.csdn.net/gzlaiyonghao/article/details/4561611)。得空便开始着手尝试，半天未果。
问题一大堆，比如：

* OS 下设置环境变量；
* Clang（/klæŋ/）安装;

OS 下终端里设置临时环境变量的方法比较容易：

```
export PATH=$PATH:'app/bin'
```

OS设置永久环境变量，网上搜了一篇，没太搞明白，但起作用了：Mac 下设置环境变量

编译 Clang 需要 GCC，GCC 我在 XCode 安装目录下找到了：


```
/Applications/Xcode.app/Contents/Developer/usr/bin
```

按照《结构化编译器前端 Clang介绍》折腾半天，最终也没能编译成功。
 
回头再去XCode安装目录：

```
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin
```


找到了，果断复制了 Clang 至：

```
/Applications/Xcode.app/Contents/Developer/usr/bin
```

也不知道，合不合适，但终端里输入 Clang 终于有反应了

``` 
cd Cython-0.17.3
python setup.py build
```

阻力继续：

```
running build_py
running build_ext
building 'Cython.Plex.Scanners' extension
clang -fno-strict-aliasing -fno-common -dynamic -g -Os -pipe -fno-common -fno-strict-aliasing -fwrapv -mno-fused-madd -DENABLE_DTRACE -DMACOSX -DNDEBUG -Wall -Wstrict-prototypes -Wshorten-64-to-32 -DNDEBUG -g -Os -Wall -Wstrict-prototypes -DENABLE_DTRACE -arch i386 -arch x86_64 -pipe -I/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7 -c Cython/Plex/Scanners.c -o build/temp.macosx-10.8-intel-2.7/Cython/Plex/Scanners.o
clang: warning: argument unused during compilation: '-mno-fused-madd'
Cython/Plex/Scanners.c:4:10: fatal error: 'Python.h' file not found
#include "Python.h"
         ^
1 error generated.
error: command 'clang' failed with exit status 1
```

stackoverflow 上有个最佳答案：

``` 
sudo apt-get install python-dev 
```

mac 里没有 apt-get 这个工具，傻眼。。。
 
其实还在 Xcode 目录中:

```
/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk/System/Library/Frameworks/Python.framework/Versions/2.7/include
```

找到了“Python.h”,怎么让 `python setup.py build` 时知道呢？再傻眼。。。
 
没有师傅指导。。。
 
难免走弯路。。。
 
会遇到些稀奇古怪的东西，完全搞不清状况。。。
 
我看到了这篇文章：《Fatal Error：studio.h File Not Found》
 
> "Command Line Tools" 的 comment：
 
> "Before installing, note that from within Terminal you can use the XCRUN tool to launch compilers and other tools embedded within the Xcode application. Use the XCODE-SELECT tool to define which version of Xcode is active.  Type "man xcrun" from within Terminal to find out more.
 
> Downloading this package will install copies of the core command line tools and system headers into system folders, including the LLVM compiler, linker, and build tools."
 
再次打开终端：输入 gcc，输入 clang，就这么简单，绕这么大弯。
 
`python setup.py build` 有些警告

``` 
sudo python setup.py install 
```

结束！