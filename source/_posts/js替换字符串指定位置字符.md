---
title: javascript 替换字符串指定位置字符
date: 2010-12-20
categories:
- iteye
tags:
- javascript
---
<!-- more -->

[原文地址](http://fiftyk.iteye.com/admin/blogs/847232)

今天遇到这个问题,首先的反应是用正则表达式,但不熟悉,pass掉了,又想到截断字符串,觉得也不好,最后想到使用循环,代码如下:

```
String.prototype.replaceAt = function(index,ch){
    var newStr = "";
    for(var i in this){
        if(i == index){
            newStr += ch;
        }
        if(typeof(this[i]) == "string"){
            newStr += this[i];
        }
    }
    return newStr;
}
```

javascript 真强大,感觉应该还有更好的办法,一时没想起来,有路过的大侠,希望能给点建议!谢谢了.