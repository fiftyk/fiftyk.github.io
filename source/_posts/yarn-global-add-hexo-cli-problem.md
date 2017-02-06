---
title: yarn global add hexo-cli 的问题
date: 2017-02-06 09:32:33
categories:
- 前端
tags:
- hexo
- yarn
---

<!-- more -->

## 问题描述

*npm* 的全局安装目录在我机器的 `/usr/local/lib/node_modules/` 目录。我手动删除了该目录下的 *hexo-cli* ，通过执行 `yarn global add hexo-cli` 后期望能重新安装 *hexo-cli*，但却没有如愿，终端执行 `hexo` 时：

```bash
> hexo
zsh: command not found: hexo
```

## 探究

实际上执行 `yarn global add hexo-cli` 时至少发生了以下几件事，也许还有其它但我还不知道：

* 首先 *hexo-cli* 安装到了 `/Users/liurtao/.config/yarn/global/node_modules/` 目录下，而不是 `/usr/local/lib/node_modules/` 目录；
* 另外在 `/usr/local/Cellar/node/7.4.0/bin/` 目录里创建了指向 `/Users/[username]/.config/yarn/global/node_modules/hexo-cli/bin/hexo` 的链接；

问题找到了，我机器的 PATH 路径并不包含 `/usr/local/Cellar/node/7.4.0/bin/` 也不包含 `/Users/[username]/.config/yarn/global/node_modules/hexo-cli/bin/`，所以在终端输入 `hexo` 时无法找到。

## 解决办法

根据官方文档 [关于 *yarn global* 的描述](https://yarnpkg.com/lang/en/docs/cli/global/) 可知，可以通过 `--prefix` 参数指定链接创建的目标路径。

```bash
# 删除原先安装
yarn global remove hexo-cli
# 重新安装
yarn global add hexo-cli --prefix /usr/local
```

执行上述代码后，我们会发现在 `/usr/local/bin/` 目录里创建了指向 `/Users/[username]/.config/yarn/global/node_modules/hexo-cli/bin/hexo` 的链接；

此时，再从终端执行 `hexo`：

```
> hexo
Usage: hexo <command>

Commands:
  help     Get help on a command.
  init     Create a new Hexo folder.
  version  Display version information.
...
```

Done!
