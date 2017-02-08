---
title: Python 参数传递
date: 2010-11-22 10:53
categories:
- qzone
tags:
- python
---

<!-- more -->

```python
def a(b,c):
    print b,c
a(1,2)
a(*[1,2])
a(**{'b':1,'c':2})
```