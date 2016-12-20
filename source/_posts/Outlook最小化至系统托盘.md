---
title: Outlook 最小化至系统托盘
date: 2010-12-28
categories:
- iteye
tags:
---
<!-- more -->
[原文](http://fiftyk.iteye.com/admin/blogs/853511)
最小化到托盘：        
Outlook启动之后最小化的时候总是在任务栏上占一个位置，工作起来很碍事，如果最小化之后能隐藏在系统托盘之中就好了，可以通过修改注册表的方法来实现，如下：

* 1. 开始菜单 -> 运行, 输入"regedit"并回车，打开注册表编辑器
* 2. 依次打开 HKEY_CURRENT_USER\Software\ Microsoft\Office\11.0（如果是 Outlook XP，此处为10.0；如果是 Outlook2007，此处是 12.0）\Outlook\Preferences
* 3. 在注册表的右边建立一个DWord的值（双字节值），名称为“MinToTray”，将值设成 1。刷新注册表。
* 4. 启动 Outlook XP/2003/2007，系统托盘区已经有一个 Outlook2007 的小图标了，再进行最小化，会发现已经不在任务栏当中，自动隐藏到系统托盘当中了。
