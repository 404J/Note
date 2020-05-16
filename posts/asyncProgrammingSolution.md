---
title: 'ES6：异步编程解决方案'
date: 2020-05-04 14:50:54
tags: [ES6]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/promises-methods.png
isTop: false
---
> 众所周知，JS中处处存在的异步操作，各种各样的 `callBack`。这使我们不论书写还是阅读代码都产生了种种的不变。为了避免过多的、依赖的异步操作造成回调地狱，`Promise` 使得我们可以链式的书写嵌套回调，`async/await` 语法糖更是可以让我们以同步的方式书写相互依赖的异步操作。又可以优雅的🐱写bug了呢！！~接下来以文件的读取这个异步操作为例，分别记录：
---
### **回调函数**
上code:
```js
const fs = require('fs')
fs.readFile('./file1.txt', (error, data) => {
    if (error) console.log('oh no', error)
    else {
        console.log(data.toString())
        fs.readFile('./file2.txt', (error, data) => {
            if (error) console.log('oh no', error)
            else {
                console.log(data.toString())
            }
        })
    }
})
```
output:
```shell
E:\ES6  (es6@1.0.0)
λ node promise.js
this is file one
this is file two
```
假如文件的读取相互依赖，又由于文件的读取属于异步操作只能靠回调函数进行文件读取后的操作，这种情况下，就会形成回调地狱🕳👻`callback hell`!!!。这种代码将会变得越来越*胖*，显然让人看起来很不舒服。

### **Promise**
`Promise` 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。
-  基本语法
```js
const promise = new Promise(function(resolve, reject) {
    // ... some async operations

    if (/* 异步操作成功 */) {
        resolve(value)
    } else {
        reject(error)
    }
})
```
`Promise` 可以使得我们摆脱无尽的回调，可以采用链式编程。接下来使用 `Promise` 解决文件的读取相互依赖问题。
```js
const fs = require('fs')
function readFilePromise (path) {
    return new Promise((resolve, reject) => {
        fs.readFile(path, (error, data) => {
            if (error) reject(error)
            else resolve(data)
        })
    })
}

readFilePromise('./file1.txt')
    .then((data) => {
        console.log(data.toString())
        return readFilePromise('./file2.txt')
    })
    .then((data) => {
        console.log(data.toString())
    })
    .catch((error) => console.log('oh no', error))
```
output:
```shell
E:\ES6  (es6@1.0.0)
λ node promise.js
this is file one
this is file two
```
从code中可以看出，如果想使用 `Promise` ，首先需要将文件读取的异步操作封装成一个 `Promise` 对象，然后，异步操作完成调用 `resolve` 函数，最后使用 `.then` 接收异步执行结果。但是，如果每次都需要手动将异步操作封装成 `Promise` 也太~~扯蛋~~麻烦了吧~好在 `Node` 中提供了 `promisify` 这个库。
```js
const fs = require('fs')
const promisify = require('util').promisify
const promisifyReadFile = promisify(fs.readFile)

promisifyReadFile('./file1.txt')
    .then((data) => {
        console.log(data.toString())
        return promisifyReadFile('./file2.txt')
    })
    .then((data) => {
        console.log(data.toString())
    })
    .catch((error) => console.log('oh no', error))
```
偷懒是程序员的美德嘛。但是其实，`Promise` 只是一个容器，一个外壳。这样好像还不是很完美！

### **async/await**
其实，`async` 和 `await` 是 [Generator](https://es6.ruanyifeng.com/#docs/generator)的语法糖，他们是建立在 `Promise` 机制上的。从字面上就可以大致理解他们的作用。接下来，基于手动封装的 `readFilePromise` 实现优雅书写代码：
```js
async function readFileAsync() {
    try {
        let data1 = await readFilePromise('./file1.txt')
        let data2 = await readFilePromise('./file2.txt')
        console.log(data1.toString())
        console.log(data2.toString())
    } catch (error) {
        console.log('oh no', error)
    }
}

readFileAsync()
```
output:
```shell
E:\ES6  (es6@1.0.0)
λ node promise.js
this is file one
this is file two
```
上面的code的逻辑是建立在*假设文件的读写存在相互的依赖关系*，即文件 `file1.txt` 和文件 `file2.txt `的读取需要串行的同步执行，假如说文件的读取没有相互的依赖关系，则可以这样实现并行处理：
```js
async function readFileAsync() {
    try {
        let [data1, data2] = await Promise.all([
            readFilePromise('./file1.txt'),
            readFilePromise('./file2.txt')
        ])
        console.log(data1.toString())
        console.log(data2.toString())
    } catch (error) {
        console.log('oh no', error)
    }
}

readFileAsync()
```


---
最后，别以为你逃离了`callBack hell`，你还有可能陷入`async/await hell`!!! 也许这篇文章可以帮助你 ---> [✨](https://www.freecodecamp.org/news/avoiding-the-async-await-hell-c77a0fb71c4c/)
OVER!🤐