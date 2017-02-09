---
title: ES6 模块加载机制
date: 2017-02-04 10:10:35
categories:
- ES6 标准入门
tags:
- es6
---
本文是《[ES6 标准入门（第 2 版）](http://es6.ruanyifeng.com/)》[第 20 章](http://es6.ruanyifeng.com/#docs/module) 的笔记。
<!-- more -->

## es6 的严格模式

以下只列举我不熟悉和不了解的:

* 不能删除变量，只能删除属性；
* eval 和 arguments 不能被重新复制；
* arguments 不能自动反映函数参数变化；
* 不能使用 arguments.caller 和 arguments.callee；
* 不能使用 fn.caller 和 fn.arguments 获取函数调用堆栈；

## 关于 export

* es6-module 是 `编译时加载`，CommonJS 是 `运行时加载`；
* export 的是代码，是值的引用，CommonJS export 的是对象，是一个值的拷贝；
* export 可以是变量、函数、类；
* export 时可以用 as 关键字重命名变量、函数、类；
* export default：

  ```javascript
  export default foo
  // 相当于
  export {foo as default}
  // 另一个文件 import 时
  import foo from './xx';
  // 相当于
  import {default as foo} from './xx';
  ```

## 关于 import

* import 命令具有提升效果
* import 时大括号中变量名必须和被导入模块对外接口的名称相同；
* import 时可以重命名被导入模块对外接口的名称，通过 as 关键字；
* import * as mod from './xxx'，整体加载一个模块，单并不包括默认导出的模块；
* import * as mod from './xxx' 等价于 module mod from './xxx';
* import 时可以同时导入默认和其它变量，import defaultVar, {otherVar} from './xxx';
* import 的变量，不能对其重新赋值；

## 导入后直接导出

```javascript
export defaultVar, {oneVar as one, otherVar} from './xxx';
export * from './xxx';
```

## 模块继承

假设 child 继承了 father 模块：

继承所有属性或方法（但不包含 default）：

```javascript
export * from father;
export let newSkill = 'new skill';
export default function(x) {
  return Math.exp(x);
}
```

## 循环加载

### 要点

* CommonJS 会在第一次 require 一个脚本时执行全部脚本，并将结果缓存在内存中；
* ES6 import 时不会执行模块，只是生成一个执行被加载模块的引用；