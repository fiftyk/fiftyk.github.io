---
title: TypeScript 模块解析策略
date: 2016-11-18 13:39:39
categories:
- 学习
tags:
- typescript
---
发表是最好的记忆。
<!-- more -->

若干天前还啃了半天英文原文[Module Resolution](http://www.typescriptlang.org/docs/handbook/module-resolution.html)，然后，今天都忘了！于是重新复习一遍，且记录下来，算是做个笔记。

# 一、相对 vs 非相对模块导入

## 1.1 相对导入

以 `/`, `./`, `../` 开头的导入：

```
import A from '/ModuleA';
import B from './ModuleB';
import C from '../ModuleC';
```

## 1.2 非相对导入

所有非相对导入之外的导入方式：

```
import * as express from 'express';
import { Component } from 'angular2/core';
```

# 二、模块导入策略

TypeScript 模块导入策略默认为 node 和 classic 两种方式。我们可以通过 `--moduleResolution` 指定模块导入策略。如果没有指定的话，tsc 会根据 `--module` 指定的值选择策略：

```
module === 'amd' | 'system' | 'ES6' ? 'classic' : 'node'
```

## 2.1 Classic

### 2.1.1 相对导入

文件 `/root/src/folder/A.ts`：

```
import { b } from "./moduleB";
```

这时编译器将搜索：

```
/root/src/folder/moduleB.ts
/root/src/folder/moduleB.d.ts
```

### 2.1.2 非相对导入时

文件 `/root/src/folder/A.ts`：

```
import { b } from "moduleB";
```

这时编译器将搜索：

```
/root/src/folder/moduleB.ts
/root/src/folder/moduleB.d.ts
/root/src/moduleB.ts
/root/src/moduleB.d.ts
/root/moduleB.ts
/root/moduleB.d.ts
moduleB.ts
moduleB.d.ts
```

## 2.2 Node 

### nodejs 的模块解析策略

#### 相对导入

文件 `/root/src/folder/A.js`：

```
var b = require("./moduleB");
```

这时编译器将搜索：

```
/root/src/folder/moduleB.js
/root/src/folder/moduleB/package.json > main
/root/src/folder/moduleB/index.ts
```

#### 非相对导入

文件 `/root/src/folder/A.js`：

```
var b = require("moduleB");
```

这时编译器将搜索：

```
/root/src/node_modules/moduleB.js
/root/src/node_modules/moduleB/package.json > main
/root/src/node_modules/moduleB/index.js
```

```
/root/node_modules/moduleB.js
/root/node_modules/moduleB/package.json > main
/root/node_modules/moduleB/index.js
```

```
/node_modules/moduleB.js
/node_modules/moduleB/package.json > main
/node_modules/moduleB/index.js
```

### 2.2.1 `--moduleResolution=node` 时，相对导入

文件 `/root/src/folder/A.ts`：

```
import B from "./moduleB";
```

这时编译器将搜索：

```
/root/src/folder/moduleB.ts
/root/src/folder/moduleB.tsx
/root/src/folder/moduleB.d.ts
/root/src/folder/moduleB/package.json > typings
/root/src/folder/moduleB/index.ts
/root/src/folder/moduleB/index.tsx
/root/src/folder/moduleB/index.d.ts
```

### 2.2.2 `--moduleResolution=node` 时，非相对导入

文件 `/root/src/folder/A.ts`：

```
import B from "moduleB";
```

这时编译器将搜索：
```
/root/src/node_modules/moduleB.ts
/root/src/node_modules/moduleB.tsx
/root/src/node_modules/moduleB.d.ts
/root/src/node_modules/moduleB/package.json > typings
/root/src/node_modules/moduleB/index.ts
/root/src/node_modules/moduleB/index.tsx
/root/src/node_modules/moduleB/index.d.ts
```

```
/root/node_modules/moduleB.ts
/root/node_modules/moduleB.tsx
/root/node_modules/moduleB.d.ts
/root/node_modules/moduleB/package.json > typings
/root/node_modules/moduleB/index.ts
/root/node_modules/moduleB/index.tsx
/root/node_modules/moduleB/index.d.ts
```

```
/node_modules/moduleB.ts
/node_modules/moduleB.tsx
/node_modules/moduleB.d.ts
/node_modules/moduleB/package.json > typings
/node_modules/moduleB/index.ts
/node_modules/moduleB/index.tsx
/node_modules/moduleB/index.d.ts
```