---
title: 'JavaScript: 闭包'
date: 2020-03-17 15:28:44
tags: [JavaScript]
published: true
hideInList: false
feature: 
isTop: false
---
在了解 js 中的闭包概念之前，首先了解以下几个概念：

- [立即执行函数](#立即执行函数)
- [作用域以及变量](#作用域以及变量)

### 立即执行函数🍑
首先理解以下概念：函数声明，函数表达式，匿名函数。如下code：
```
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
```
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
ES5 只有全局作用域和函数作用域，全局作用域只的是script标签或者一个js文件。函数作用域指的是，函数被调用时，在内存中创建函数作用域，函数执行完毕后，该作用域被销毁，且函数每调用一次，就会创建一个新的函数作用域，相互独立。当在函数作用域操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用，如果没有则向上一级作用域中寻找，直到找到全局作用域，如果全局作用域中依然没有找到，则会报错ReferenceError。
```
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
[参考文档](https://zhuanlan.zhihu.com/p/22486908)


