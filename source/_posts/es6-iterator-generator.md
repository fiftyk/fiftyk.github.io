---
title: Iterator & Generator
date: 2017-02-14 14:03:31
categories:
- ES6 标准入门
tags:
- es6
- Iterator
- Generator
---
本文是《[ES6 标准入门（第 2 版）](http://es6.ruanyifeng.com/)》[第 15 章](http://es6.ruanyifeng.com/#docs/iterator) 和 [第 16 章](http://es6.ruanyifeng.com/#docs/generator) 的笔记。
<!-- more -->

> Generator 函数返回一个遍历器对象。

```javascript
// Generator 函数
function* gen(){
  yield 1;
}
// 执行 Generator 函数返回一个遍历器对象
var g = gen();
// 遍历器对象也有一个 Symbol.iterator 方法，执行后返回自身
g[Symbol.iterator]() === g;
```

> 一旦 next 方法返回对象的 done 属性为 true 时， for of 循环就会终止，且不包含该返回对象。

```javascript
function* gen() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  return 5;
}

[...gen()]//1,2,3,4
```

> Generator 函数返回的遍历器对象都有一个 throw 方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

```javascript
```

> Generator 函数返回的遍历器对象都有一个 return 方法，可以返回给定的值，并终结 Generator 函数的遍历

```javascript
```

> Generator 函数内部如果有 try ... finally 代码块，那么 return 方法会推迟到 finally 代码块后执行完再执行。

```javascript
function* gen() {
  yield 0;
  try {
    yield 1;
    yield 2;
  } finally {
    yield 3;
    yield 4;
  }
}

var g = gen();

g.next();       //{value: 0, done: false}
g.next();       //{value: 1, done: false}
g.return(7);    //{value: 3, done: false}
g.next();       //{value: 4, done: false}
g.next();       //{value: 7, done: true}

var g0 = gen();

g0.next();      //{value: 0, done: false}
g0.next();      //{value: 1, done: false}
g0.return(7);   //{value: 3, done: false}
g0.return(8);   //{value: 8, done: done}
g0.next();      //{value: undefined, done: true}

var g1 = gen();

g1.return(7);   //{value: 7, done: true}
g1.return(8);   //{value: 8, done: true}
g1.next();      //{value: undefined, done: true}

var g2 = gen();

g2.next();     //{value: 0, done: false}
g2.return(7);  //{value: 7, done: true}
g2.return(8);  //{value: 8, done: true}
g2.next();     //{value: undefined, done: true}
```

> yield* 不过是 for...of 的一种简写形式，完全可以用后者替代。

```javascript
function* concat(iter1, iter2) {
  yield* iter1;
  yield* iter2;
}
```

等价于：

```javascript
function* concat(iter1, iter2) {
  for (let value of iter1) {
    yield value;
  }
  for (let value of iter2) {
    yield value;
  }
}
```

> yield* 语句遍历完全二叉树。

```javascript
```

> 如果被代理的 Generator 函数有 return 语句，那么可以向代理它的 Generator 函数返回数据

```javascript
function* foo() {
  yield 1;
  yield 2;
  return 3;
}

function* bar() {
  yield 'a';
  var x = yield* foo();
  yield x;
}

[...bar()]// 'a', 1, 2, 3
```

