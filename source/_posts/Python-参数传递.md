---
title: Python 参数传递
date: 2016-12-20 15:53:16
categories:
tags:
- python
---

<!-- more -->

```
def a(b,c):
    print b,c
a(1,2)
a(*[1,2])
a(**{'b':1,'c':2})
```