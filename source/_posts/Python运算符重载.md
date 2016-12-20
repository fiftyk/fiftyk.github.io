---
title: Python 运算符重载
date: 2012-12-18
categories:
- iteye
tags:
- python
---
<!-- more -->

[原文地址](http://fiftyk.iteye.com/admin/blogs/1749772)

```
class Line:
  def __init__(self,p1,p2):
    self.start = p1
    self.end = p2

  def __sub__(self,p):
    if isinstance(p,Point):
      if p is self.start:
        return self.end
      if p is self.end:
          return self.start

class Point:
  def __init__(self,x,y):
    self.x = x
    self.x = x

  def __add__(self,p):
    if isinstance(p,Point):
      return Line(self,p)

if __name__ == '__main__':
  p1 = Point(1,2)
  p2 = Point(2,3)

  #两个点相加 通过__add__方法
  line = p1 + p2 
  print l
  #>><__main__.Line instance at 0x02220738>
  #线减去一个点 通过__sub__方法
  print line - p1 
  #>><__main__.Point instance at 0x02220710>
```