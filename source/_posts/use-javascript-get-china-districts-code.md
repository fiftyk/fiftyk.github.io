---
title: 使用 Javascript 获取国家统计局网站行政区划代码
date: 2016-12-30 17:11:51
categories:
- 杂项
tags:
- javascript
---
使用 Javascript 获取国家统计局网站行政区划代码，并组织生成树状结构。
<!-- more -->

项目需要需要一份中国的行政区划代码，以及上下级关系。数据源从国家统计局网站上的[最新县及县以上行政区划代码（截止2015年9月30日）](http://www.stats.gov.cn/tjsj/tjbz/xzqhdm/201608/t20160809_1386477.html)获取。

代码如下：

[fiftyk/districts-convert.js](https://gist.github.com/fiftyk/71a3b5949511cf6320939e7115aa6645)

```javascript
(function(results) {
  let els = $('p.MsoNormal');
  //
  for (let i = 0, size = els.length, prev; i < size; i++) {
    let text = $(els[i]).text();
    let ary = text.split(/\s+/);
    let code = ary[0];
    let name = ary[1];
    let level = text.split(/\s/).length - 1;
    //
    let current = {
      code, name, level
    };
    //
    if(!prev) {
      //
    } else if (current.level > prev.level) {
      current.parent = prev;
    } else if (current.level <= prev.level) {
      let parent = prev;
      //
      while(parent && parent.level >= current.level ) {
        parent = parent.parent;
      }
      //
      if(parent) {
        current.parent = parent;
      }
    }
    //
    prev = current;
    //
    results.push(current);
  }
})(window.results=[])
```