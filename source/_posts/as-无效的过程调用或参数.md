---
title: 无效的过程调用或参数
date: 2010-07-16 13:41:14
categories:
- douban
tags:
- actionscript
---

<!-- more -->

[原文地址](https://www.douban.com/note/80888634/)

我将 actionscript 中的一个名为 dir 的方法,通过 ExternalInterface.addCallback(js_function_name,as_function) 开放给 JavaScript 调用, FireFox 下没有错误，IE8 下却报错,但不影响其程序其他部分运行。