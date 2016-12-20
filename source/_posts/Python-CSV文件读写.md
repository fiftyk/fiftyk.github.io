---
title: Python-CSV文件读写
date: 2013-01-08
categories:
- iteye
tags:
- python
---

[原文地址](http://fiftyk.iteye.com/admin/blogs/1766530)

之前也曾使用过 python 处理过 csv 文件，就是普通的文本文件读写，不曾想到 Python 也内置了一个 csv 的库：
 
[http://docs.python.org/release/2.5.4/lib/module-csv.html](http://docs.python.org/release/2.5.4/lib/module-csv.html)
 
没有细看，有空再看:

```
import csv
reader = csv.reader(open("some.csv", "rb"))
for row in reader:
    print row
reader = csv.reader(open("passwd", "rb"), delimiter=':', quoting=csv.QUOTE_NONE)
for row in reader:
    print row
writer = csv.writer(open("some.csv", "wb"))
writer.writerows(someiterable)
csv.register_dialect('unixpwd', delimiter=':', quoting=csv.QUOTE_NONE)
reader = csv.reader(open("passwd", "rb"), 'unixpwd')
filename = "some.csv"
reader = csv.reader(open(filename, "rb"))
try:
    for row in reader:
        print row
except csv.Error, e:
    sys.exit('file %s, line %d: %s' % (filename, reader.line_num, e))
for row in csv.reader(['one,two,three']):
    print row
```