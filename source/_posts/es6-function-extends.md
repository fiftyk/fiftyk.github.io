---
title: 函数的扩展
date: 2017-02-09 15:27:25
categories:
- ES6 标准入门
tags:
- es6
---
本文是《[ES6 标准入门（第 2 版）](http://es6.ruanyifeng.com/)》[第 8 章](http://es6.ruanyifeng.com/#docs/function) 的笔记。
<!-- more -->

## 函数参数的默认值

```javascript
function foo(x, y=5) {
  console.log(x, y);
}
foo();//undefined, 5
foo(1);//1, 5
foo(1, 3);//1, 3
```

```javascript
function foo({x, y=5}) {
  console.log(x, y);
}
foo();//TypeError
foo({x: 1});//1, 5
foo({y: 3});//undefined, 3
foo({x: 1, y: 3});//1, 3
```

```javascript
function foo({x, y=0}={}) {
  console.log(x, y);
}
foo();//undefined, 0
```

```javascript
function foo(x, y=5, z) {
  console.log(x, y, z);
}
foo();//undefined, undefined, undefined
foo(1, undefined, 7);//1, 5, 7
foo(1, null, 7);//1, null, 7
```

*如果参数的默认值是一个变量，则该变量所处的作用域与其他变量的作用域规则一致，即先是当前函数的作用域，然后才是全局作用域。*

```javascript
function foo(x, y=x) {
  console.log(x, y);
}
foo();//undefined, undefined

var x = 2;
function foo(x, y=x) {
  console.log(x, y);
}
foo();//undefined, 2
foo(1);//1, 1

let x = 2;
function foo(y=x) {
  let x = 1;
  console.log(x, y);
}
foo();//1, 2
foo(1);//1, 1
```

## rest 参数

```javascript
function foo(a, b, ...c) {
  console.log(a, b, c);
}
foo(1, 2, 3, 4);//1, 2, [3, 4]

function foo(a, ...b, c) {
  console.log(a, b, c);
}
foo(1, 2, 3, 4);//SyntaxError
```

*应用：*

```javascript
function throwIfMiss() {
  throw new Error('you miss some paramter');
}

function foo(a=throwIfMiss()){
  console.log(a);
}
foo();//Error: you miss some paramter
```

## 扩展运算符

```javascript
function foo(a, b) {
  console.log(a, b);
}
foo(...[1, 2]);//1, 2
```

合并数组

```javascript
a = [1, 2];
b = [3, 4];
a.concat(b);//[1, 2, 3, 4]
[1, 2, ...b];//a = [1, 2];
```

分隔字符串

```javascript
s = 'abc';
[...s];//['a', 'b', 'c']
```

解构赋值：

```javascript
let [a, ...b, c] = [1, 2, 3, 4];
console.log(a, b, c);//SyntaxError
let [a, b, ...c] = [1, 2, 3, 4];
console.log(a, b, c);//1, 2, [3, 4]
let [...a, b, c] = [1, 2, 3, 4];
console.log(a, b, c);//SyntaxError
```

## 函数的 length 属性值：

```javascript
function abc(x, y=5, z) {
  console.log(x, y, z);
}
abc.length;//2

function foo(x, y, z) {
  console.log(x, y, z);
}
foo.length;//3

function bar(...reset) {
  console.log(...reset);
}
bar.length;//0
```

## name 属性

```javascript
(new Function).name;//'anonymous'
(new Function).bind().name;//'bound anonymous'
(function(){}).name;//''
(function(){}).bind().name;//'bound '

(function foo(){}).name;//'foo'
(function foo(){}).bind().name;//'bound foo'
```

## 箭头函数

> ① `this` 指向的固化，并不是因为箭头函数内部有绑定 `this` 的机制，实际原因是箭头函数根本没有自己的 `this`，导致内部的 `this` 就是外层代码块的 `this`。正因为它没有 `this`，所以也就不能用作构造函数。

> ② 箭头函数中不存在 `arguments`，`super`，`new.target`。

## 函数绑定

ES7 中 `::` 运算符。

```javascript
function foo() {
  console.log(this.name);
}
let bar = {name: 'xyz'};
foo.call(bar);
//可写成
bar::foo();//'xyz'
```

* 如果 `::` 左边为空，右边为一个对象的方法，则等于将该方法绑定到对象上

```javascript
let method = obj::obj.method;
//等价于
let method = ::obj.method;

let console = ::console.log;
//等价于
let console = console.log.bind(console);
```

* ~~运算符 `::` 返回的还是原对象；~~（??不理解这句话的意思）

## 尾调用优化

`尾调用` 是 `函数式编程` 的一种重要概念... 又提及了 `函数柯里化`...
