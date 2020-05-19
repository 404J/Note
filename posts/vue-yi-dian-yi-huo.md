---
title: 'Vue: 一点疑惑'
date: 2020-05-19 10:19:54
tags: []
published: true
hideInList: false
feature: 
isTop: false
---
[英文文档](https://vuejs.org/v2/style-guide#Component-name-casing-in-templates-strongly-recommended)
[中文文档](https://cn.vuejs.org/v2/style-guide/#%E8%87%AA%E9%97%AD%E5%90%88%E7%BB%84%E4%BB%B6%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90)
[参考文档1](https://segmentfault.com/a/1190000014888725)
[参考文档2](https://forum.vuejs.org/t/confused-about-dom-template-and-string-template/1797)

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