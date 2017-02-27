---
title: 对象的扩展
date: 2017-02-10 15:47:08
categories:
- ES6 标准入门
tags:
- es6
---
本文是《[ES6 标准入门（第 2 版）](http://es6.ruanyifeng.com/)》[第 9 章](http://es6.ruanyifeng.com/#docs/object) 的笔记。
<!-- more -->

对象的扩展总结下来有以下几处：

* 属性的简洁表示法；
* 属性表达式；
* 方法的 name 属性；
* Object.is；
* Object.assign；
* Object.setPrototypeOf & Object.getPrototypeOf；
* 对象属性的枚举性以及遍历；
* 对象扩展运算符；

## 属性的简洁表示法

```javascript
let foo = {bar}
// 等价于
let foo = {bar: 'bar'}

let foo = {bar(){}}
// 等价于
let foo = {bar: function bar(){}}
```

## 属性表达式

```javascript
let foo = {['bar' + 0]: function (){}}
// 等价于
let foo = {bar0: function (){}}
// 但，下面不可以
let foo = {['bar' + 0](){}}
```

属性表达式不可以同属性简洁表示法混用。

## 方法的 name 属性

这个和 function 的 name 属性一样。

```javascript
let foo = {['bar' + 0]: function (a, b, ...c){}}
foo.bar0.name// 'bar0'
// 类比一下函数的 length 属性
foo.bar0.length// 2
```

## Object.is

判断两个值是否严格相等:

```javascript
Object.is(null, null)// true
Object.is(undefined, undefined)// true
Object.is(true, true)// true
Object.is('1', '1')// true
Object.is(1, 1)// true
Object.is(NaN, NaN)// true

Object.is(-0, +0)// false
Object.is({}, {})// false
```

## Object.assign(target, ...sources)

[MDN: Object.assign()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

几点需要明确：

* copy 所有 string 和 symbol 属性；
* 仅 copy 自身属性和可枚举属性；
* `It uses [[Get]] on the source and [[Set]] on the target`;
* target 如果是原始类型，会被先转成对象；

```javascript
var s = {s: 1}
var so = Object.assign(s, null,{a: 1})
s === so// true

var s = 1
var so = Object.assign(s, null,{a: 1})
s === so// false
```

## Object.setPrototypeOf & Object.getPrototypeOf

```javascript
Object.setPrototypeOf(foo, bar);
// 相当于
function setPrototypeOf(foo, bar) {
  foo.prototype = bar;
  return foo;
}
```

## 对象属性的枚举性以及遍历

* for in
* Object.keys
* Object.getOwnPropertyNames
* Object.getOwnPropertySymbols
* Reflect.ownKeys
* ~~Reflect.enumerate~~

```javascript
let parent = {
  p1: 'p1', 
  [Symbol('p2')]: 'p2'
};

Object.defineProperties(parent, {
  'p3': {
    value: 'p3',
    enumerable: false,
    writable: true,
    configurable: true
  }
});

let child = {
  c1: 'c1', 
  [Symbol('c2')]: 'c2'
};

Object.defineProperties(child, {
  'c3': {
    value: 'c3',
    enumerable: false,
    writable: true,
    configurable: true
  }
});

Object.setPrototypeOf(child, parent);

for (let key in child)
  console.log(key)
// > c1 
// > p1

Object.keys(child)
// > ["c1"]

Object.getOwnPropertyNames(child)
// > ["c1", "c3"]

Object.getOwnPropertySymbols(child)
// > [Symbol(c2)]

Reflect.ownKeys(child);
// > ["c1", "c3", Symbol(c2)]
```

### 遍历次序规则

* 数字属性优先，安装数字大小；
* 再字符串，按生成顺序；
* 最后 symbol 属性，按生成顺序；

```javascript
let parent = {
  1: 'p1', 
  [Symbol('p2')]: 'p2',
  [Symbol('p0')]: 'p0',
  0: '0',
  '---': '---',
  '-2': '-2',
  '100': '100',
  '20': '20',
  '----': '----',
  '-100': '-100', 
  'z': 'z',
  'a': 'a'
};

Reflect.ownKeys(parent);
// > ["0", "1", "20", "100", "---", "-2", "----", "-100", "z", "a", Symbol(p2), Symbol(p0)]
```

## 对象扩展运算符

ES7 的提案。

```javascript
let {x, y, ...z} = {x: 1, y: 2, z: 3, a: 4}
x// 1
y// 2
z// {z: 3, a: 4}
```

> Generator 函数总是返回一个遍历器， ES6 规定这个遍历器是 Generator 函数的实例，也继承了 Generator 函数 prototype 对象上的方法。

```javascript
function* gen() {}

gen.prototype.hi = function() {
  console.log('hi');
};

let g = gen();

g instanceof gen;           // true
gen[Symbol.hasIntance](g);  // true
g.hi();                     // 'hi'
```

> 参数的求值策略：传值调用 `call by value`， 传名调用 `call by name`。

```javascript
function foo(x, y) {
  return y;
}
var a = 1;
var b = 2;

foo(a + 5, b);
```

传值调用的话，即先计算 `a + 5`, `foo(a + 5, b)` 即 `foo(6, 2)`。传名调用也有它的好处，对上面的 foo 函数而言我们不用计算 `a + 5`，因为这个参数不影响最终结果，没有被使用到。传值调用为我们省去了不必要的计算。

### Thunk 函数

> Thunk 函数是自动执行 Generator 函数的一种方法。

Thunk 函数是一种 "传名调用" 的实现方式，Javascript 里我们通过将语句包装成函数来实现：

```javascript
// 将任意参数有回调函数的方法，转换为 Thunk 形式
function Thunk(fn) {
  return function(...args) {
    return function(callback){
      fn.call(this, ...args, callback)
    }
  }
}

var ThunkReadFile = Thunk(fs.readFile);
ThunkReadFile(file)(callback);
```

基于 Thunk 的 Generator 函数自动调度：

```javascript
var gen = function* (){
  yield ThunkReadFile(fileA);
  yield ThunkReadFile(fileB);
  //..
  yield ThunkReadFile(fileN);
};

function run (gen) {
  var g = gen();

  function next(err, data) {
    let result = g.next();
    if(result.done)(
      return;
    )
    result.value(next);
  }

  next();
}
```

基于 Promise 的 Generator 函数自动调度：

```javascript
var read = function(file) {
  return new Promise(function(resolve, reject) {
    fs.readFile(file, (error, data) > {
      if (error) {
        reject(err);
      } else{
        resolve(data);
      }
    })
  });
}

var gen = function* (){
  yield read(fileA);
  yield read(fileB);
  //..
  yield read(fileN);
};

function run(gen) {
  var g = gen();

  function next() {
    var result = g.next();
    if(result.done) {
      return;
    }
    result.value.then(next);
  }

  next();
};
```

> async 函数返回一个 Promise 对象。

> async 函数内部的 return 语句返回的值，会成为 then 方法回调函数的参数。

await 命令后面是一个 promise 对象。