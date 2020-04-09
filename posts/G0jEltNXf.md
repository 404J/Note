---
title: 'JavaScript : this'
date: 2020-03-19 21:33:12
tags: [JavaScript]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/javascript_this.jpg
isTop: false
---
**Yanina 👧 is very beautiful, and she is also very slim!**
```js
var person = {
    firstName: 'Mars',
    lastName: 'Shi',
    fullName: function () {
        console.log(this.firstName, this.lastName)
    }
}

person.fullName()
```
> 以上文中的英语语句和js code进行对比，来初步理解JavaScript中的this用法。其中*Yanina*和
> *person*映射，*she* 和*this*映射。在英文语句中，*she*用来代替上下文中的*Yanina*,
> *this*关键字是用来指代那个被当前函数（就是使用了 this 的函数）绑定的对象*person*。
> *this*其实就是一个具有调用当前函数的对象的值的变量。

## 全局作用域使用this 😉(非Node环境)
```js
var firstName = "Yanina"
var lastName = "Bu"
function fullName () {
    console.log(this.firstName, this.lastName)
}

var person = {
    firstName: 'Mars',
    lastName: 'Shi',
    fullName: function () {
        console.log(this.firstName, this.lastName)
    }
}

fullName() // Yanina Bu
this.fullName() // Yanina Bu
person.fullName() // Mars Shi
```
> 以上code中，*firstName*, *lastName*和*fullName*都是定义在全局作用域的变量。全局定义的
> 函数中，this指向*window*对象。但是`person.fullName()`中this的**上下文**为*person*
> 对象，所以，this指向*person*

在下面这些情景中， this 关键字可能会变得十分难以理解。在示例中同时给出了解决有关 this 使用错误的方案。
#### 1. 包含 this 的方法被当做回调函数时遇到的问题🤑

error:
```js
var person = {
    firstName: 'Mars',
    lastName: 'Shi',
    fullName: function () {
        console.log(this.firstName, this.lastName)
    }
}
setTimeout(person.fullName, 1000)
```
> `person.fullName`作为*setTimeout*的回调函数，此时的*fullName*函数执行的上下文为
> *Timeout*对象，所以此时this指向的对象为*Timeout*对象。

correct:
```js
var person = {
    firstName: 'Mars',
    lastName: 'Shi',
    fullName: function () {
        console.log(this)
        console.log(this.firstName, this.lastName)
    }
}
setTimeout(person.fullName.bind(person), 1000)
```
> 使用bind()方法显式的设置this的值。

#### 2. this 出现在闭包内遇到的问题😧

error:
```js
var clazz = {
    clazzName: 'class No.1',
    students: [
        'Mars', 'Yanina'
    ],
    call: function () {
        this.students.forEach(function (student) {
            console.log(student, 'from', this.clazzName)
        })
    }
}

clazz.call()
```
> *forEach*中的匿名函数为*call*的内层函数，内层函数中不可访问外层函数的this变量

correct:
```js
var clazz = {
    clazzName: 'class No.1',
    students: [
        'Mars', 'Yanina'
    ],
    call: function () {
        var clazzObj = this
        this.students.forEach(function (student) {
            console.log(student, 'from', clazzObj.clazzName)
        })
    }
}

clazz.call()
```
or
```js
var clazz = {
    clazzName: 'class No.1',
    students: [
        'Mars', 'Yanina'
    ],
    call: function () {
        this.students.forEach((student) => {
            console.log(student, 'from', this.clazzName)
        })
    }
}

clazz.call()
```
> 方法一使用变量将this'转存'。方法二，使用*ES6*的箭头函数，箭头函数内部的this是词法作用域，
> 由上下文确定，本例中的上下文是*clazz*

#### 3. 把一个 this 方法 赋给一个变量时出现的问题😌

error:
```js
var students = ['mars', 'yanina']
var clazz = {
    clazzName: 'class No.1',
    studesnts: [
        'Mars', 'Yanina'
    ],
    call: function () {
        this.students.forEach((student) => {
            console.log(student)
        })
    }
}
var callFromClass1 = clazz.call
callFromClass1()
```
> 此时`callFromClass1()`取的*studesnts*不是*clazz*中的属性，而是全局的*clazz*，因为函
> 数*callFromClass1*的执行上下文是全局。

correct:
```js
var callFromClass1 = clazz.call.bind(clazz)
callFromClass1()
```
> 使用`bind()`方法将*call*和*clazz*对象绑定起来，显式的设置*this*的值。

#### 4. 当借用方法的时候 this 的值不正确的问题🙃

error:
```js
var gameController = {
	scores: [20, 34, 55, 46, 77],
	avgScore: null,
	players: [
		{name: "Tommy", playerID: 987, age: 23},
		{name: "Pau", playerID: 87, age: 33}
	]
}
var appController = {
	scores: [900, 845, 809, 950],
	avgScore: null,
	avg: function() {
		var sumOfScores = this.scores.reduce(function(prev, cur, index, array) {
			return prev + cur
		})
		this.avgScore = sumOfScores / this.scores.length
	}
}
gameController.avgScore = appController.avg()
console.log(gameController.avgScore)
console.log(appController.avgScore)
```
> 在 avg 方法中的 this 不会指向 gameController 对象，而会指向 appController 对象，因
> 为它是被 appController 对象所调用的。

correct:
```js
appController.avg.apply(gameController, gameController.scores)
```
> gameController 对象借用了 appController 的 avg() 方法。在 appController.avg() 中
> 的 this 的值会被设置成 gameController 对象，因为我们把 gameController 作为第一个参数
> 传入了 apply() 方法中。传入 apply() 方法的第一个参数会被显式地设置为 this 的值
