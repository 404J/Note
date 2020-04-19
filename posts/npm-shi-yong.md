---
title: 'NPM 使用'
date: 2020-04-18 17:38:47
tags: [NPM,Node,JavaScript]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/npm.png
isTop: false
---
#### 安装
一般在安装`Node`之后，NPM作为`Node`的默认包管理器，已经被安装好了。可以执行 `npm -v`查看当前`NPM`版本
```shell
λ npm -v
6.9.0
```
---
#### 使用NPM安装依赖包
以 `express`为例子。执行该命令的时候，首先会在当前目录检查是否有`node_modules`该文件夹，没有则创建。然后在`node_modules`目录下创建`express`目录。在代码中直接`require('express')`即可引用该包。

`Node`中，使用`require`可以引用🔽
1. 核心模块即`Node`内置的模块如fs, `require('fs')`；
2. 文件模块即dev自己开发的模块，`require('../hello.js')`；
3. 第三方模块，如`express`, `require('express')`, `Node`会首先查询当前文件目录下的`node_modules`目录，所以npm进行包管理和`Node`模块引用是相辅相成的。
```shell
λ npm install express
...
+ express@4.17.1
added 50 packages from 37 contributors and audited 126 packages in 6.899s
found 0 vulnerabilities
```
---
#### 使用NPM移除依赖包
对上面安装的`express`包进行移除
```shell
λ npm uninstall express
...
removed 1 package and audited 160 packages in 1.452s
found 0 vulnerabilities
```
---
#### 发布包
简单记录如何将自己开发的模块发布到`NPM`仓库中并通过`NPM`安装。
1. 创建简单的模块`hello.js`
```js
exports.printHello = function () {
  console.log('hello nodejs')
}
```
2. 执行`npm init `创建包描述文件`package.json`
```shell
λ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (hellonpm) hello_npm_404
version: (1.0.0)
description: A hello npm test
entry point: (hello.js)
test command:
git repository:
keywords:
author: 404
license: (ISC)
About to write to C:\Users\mars.shi\Desktop\helloNPM\package.json:

{
  "name": "hello_npm_404",
  "version": "1.0.0",
  "description": "A hello npm test",
  "main": "hello.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "404",
  "license": "ISC"
}


Is this OK? (yes) yes
```
3. 注册包仓库账号
```shell
λ npm adduser
Username: 404j
Password:
Email: (this IS public) j1491361626@gmail.com
Logged in as 404j on https://registry.npmjs.org/.
```
> 需要`npm`官网验证邮箱呦
1. 上传包
```shell
λ npm publish
...
+ hello_npm_404@1.0.0
```
#### 安装包
切换至另一个目录，执行
```shell
λ npm install hello_npm_404
...
+ hello_npm_404@1.0.0
added 1 package from 1 contributor and audited 1 package in 2.674s
found 0 vulnerabilities
```
这样在这个目录就可以引用自己开发的`NPM`包了
```js
const hello = require('hello_npm_404')
console.log(hello.printHello())
```
