---
title: 'JavaScript: 闭包'
date: 2020-03-17 15:28:44
tags: [JavaScript]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/shahadat-rahman-BfrQnKBulYQ-unsplash.jpg
isTop: false
---
在了解 js 中的闭包概念之前，首先了解以下几个概念：

- [立即执行函数](#立即执行函数)
- [作用域以及变量](#作用域以及变量)

### 立即执行函数🍑
首先理解以下概念：函数声明，函数表达式，匿名函数。如下code：
```js
function fun () {
    console.log("函数声明")
}
var f = function fun () {
    console.log("函数表达式")
}
function () {
    console.log("匿名函数")
}
```
立即执行函数即为函数表达式在创建后立即执行。实现方式如下：
```js
(function (param) {
    console.log(param) // 输出 1，使用()运算符
} ) (1)

(function (param) {
console.log(param) // 输出 1，使用()运算符
} (1) )

[! || - || +]function (param) {
console.log(param) // 输出 1 ，使用 ! 或 - 或 + 运算符
} (1)
```

### 作用域以及变量🍔
ES5 只有全局作用域和函数作用域，全局作用域指的是script标签或者一个js文件。函数作用域指的是，函数被调用时，在内存中创建函数作用域，函数执行完毕后，该作用域被销毁，且函数每调用一次，就会创建一个新的函数作用域，相互独立。当在函数作用域操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用，如果没有则向上一级作用域中寻找，直到找到全局作用域，如果全局作用域中依然没有找到，则会报错ReferenceError。
```js
var param = '全局变量param'
function func1 () {
  var param1 = 'func1的局部变量param1'
  function func2 () {
    var param2 = 'func1的局部变量param2'
    console.log(param2) // func1的局部变量param2
    console.log(param) // 全局变量param
  }
  func2()
  console.log(param) // 全局变量param
  console.log(param1) // func1的局部变量param1
  console.log(param2) // ReferenceError: param2 is not defined
}

func1()
```
在函数作用域中可以访问到全局作用域的变量，在全局作用域中无法访问到函数作用域的变量

# 开始弄懂闭包🍖
## 1. 什么是闭包？😮
```js
(function () {  // 立即执行函数
    var localParam = "局部变量"
    function func () {
        console.log(localParam)
    }
})()

闭包在 ES6 体现：
{
    let localParam = "局部变量"
    function func () {
        console.log(localParam)
    }
}
```
上面code中，立即执行函数中的三行代码: 一个局部变量，一个可以访问这个局部变量的函数，这两个条件的总和（或者叫做环境）就是闭包。

## 2. 闭包的作用是什么？😜
假如说，一个公司的员工的基础工资为1000💰，但是作为老板的你不希望其他leader随随便便就给手下的员工涨工资，需要经过你的审核。那么下面方案就符合你的要求：
```js
(function () {
    var employeeSalary = 1000
    function riseSalary (amount) {
        console.log("老板审核完成，允许涨工资")
        employeeSalary += amount
        return employeeSalary
    }
    globalThis.riseSalary = riseSalary
})()
console.log(globalThis.riseSalary(100))
```
如果想给员工涨工资，就必须调用`riseSalary()`函数，小leader就不能随随便便操作`employeeSalary`。
其实我理解的闭包的作用就是，有那么一个变量，你不希望把它暴漏在全局作用域，从而让别人直接访问到。那么你会把这个变量声明为局部变量，但是局部变量别人就访问不到了。那么此时就需要使用闭包，提供一个访问器（函数），让使用者间接使用这个局部变量。

## 3. 小结⏳
闭包看起来不就是想让`var`命令声明的变量不可以在全局访问到吗？！😂那就用 ES6 的`let`和`const`来声明变量吧！
最后发现一篇挺好的文章，丢个链接[👍](https://www.cnblogs.com/zhuzhenwei918/p/6131345.html)
