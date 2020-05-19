---
title: 'Vue: 一点疑惑'
date: 2020-05-19 10:19:54
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
[问题发现](http://404j.fun/post/vue-yi-dian-yi-huo/)
[vue官方建议](https://vuejs.org/v2/style-guide/#Self-closing-components-strongly-recommended)
[对于Dom模板和字符串模板的解释](https://forum.vuejs.org/t/confused-about-dom-template-and-string-template/1797)
[vue issue](https://github.com/vuejs/vue/issues/1036)
[Dom模板如何工作](https://stackoverflow.com/questions/56941466/vue-component-not-mounting-or-rendering-and-no-error-messages/56941500)
[为啥尽量避免Dom模板而用字符串模板](https://vuejsdevelopers.com/2017/09/17/vue-js-avoid-dom-templates/)



```html
<!DOCTYPE html>
<html lang="en">

<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>

<body>
    <div id="app">
        <my-component></my-component>
        <!-- <my-component /> -->
        能看到我吗？
    </div>
</body>
<script>
    let MyComponent = {
        template: `
            <div>my test component</div>
        `
    } 
    const app = new Vue({
        el: '#app',
        components: {
            'my-component': MyComponent
        }
    })
</script>

</html>
```