---
title: 'JavaScript : this'
date: 2020-03-19 21:33:12
tags: [JavaScript]
published: true
hideInList: false
feature: 
isTop: false
---
**Yanina is very beautiful, and she is also very slim!**
```
var person = {
    firstName: 'Mars',
    lastName: 'Shi',
    fullName: function () {
        console.log(this.firstName, this.lastName)
    }
}

person.fullName()
```
以上文中的英语语句和js code进行对比，来初步理解JavaScript中的this用法。
> 🍋












