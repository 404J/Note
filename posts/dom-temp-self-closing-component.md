---
title: 'Vue: Dom 模板中的自闭和的自定义组件'
date: 2020-05-19 10:19:54
tags: [Vue]
published: true
hideInList: false
feature: 
isTop: false
---

> 初学 `Vue2️⃣▪️0️⃣` ，遇到个小问题，以此记录，no bb，coding ⬇️

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <my-component />
        Can u see me ?
    </div>
</body>
<script>
    let MyComponent = {
        template: `
            <div>
                My custom component
            </div>
        `
    } 
    const app = new Vue({
        el: '#app',
        components: {
            MyComponent
        }
    })
</script>
</html>
```
在这不足 ~~20~~30行的 `Vue` demo中，定义了一个 `MyComponent` 的自定义组件，然后在 `Vue` 实例挂载的节点 `#app` 中使用了该组件。紧接着在这个组件标签的后面，写了这么一段话 **Can u see me ?**，但是在浏览器中并没有显示。。。
![](http://404j.fun/post-images/1589957417336.png)
F12 检查下：
![](http://404j.fun/post-images/1589955661234.png)
可以看到，**Can u see me ?** 并没有被渲染。WF?

然后到 `Vue` 官网search下，这么说的：
![](http://404j.fun/post-images/1589956289683.png)
出现了这么几个术语  `string templates` 、`DOM templates`。然后有search了一波[Confused about DOM template and string template](https://forum.vuejs.org/t/confused-about-dom-template-and-string-template/1797)：
- string templates: 使用 `template` 渲染或 `.vue` 文件
- DOM templates: 使用 `el` 挂载html标签

原文中解释 `DOM templates` 时候，一句关键的语句：
> This HTML will be controlled by the browser before Vue can work with it

OK, 所以上文的demo属于 `DOM templates` , 且根元素 `#app` 中的 `<my-component />` 首先被浏览器渲染，然后交给Vue处理。
但是！
**Unfortunately, HTML doesn’t allow custom elements to be self-closing - only official “void” elements. **
而且，尤大大也不建议把自定义组件写成自闭和的形式[Self-closing component tags](https://github.com/vuejs/vue/issues/1036)
所以我应该这么写：
```html
<body>
    <div id="app">
        <!-- <my-component /> -->
        <my-component></my-component>
        Can u see me ?
    </div>
</body>
```
如你所愿，页面上貌似正常了：
![](http://404j.fun/post-images/1589957461306.png)

但是，假如这样写 `<my-component />` 浏览器会把它处理成啥样，Vue模板编译器最终拿到的是什么样子的呢? Stackoverflow上有这么一篇相关的文章[Vue component not mounting or rendering and no error messages
](https://stackoverflow.com/questions/56941466/vue-component-not-mounting-or-rendering-and-no-error-messages/56941500)
是这么说的：
![](http://404j.fun/post-images/1589958230908.png)
所以说这种写法，
```html
<body>
    <div id="app">
        <my-component />
        Can u see me ?
    </div>
</body>
```
等同于：
```html
<body>
    <div id="app">
        <my-component>
            Can u see me ?
        </my-component>
    </div>
</body>
```
所以，当 `Vue` 拿到这样的模板后，`Can u see me ?` 理所当然不会被渲染出来，因为在 `MyComponent` 组件定义的时候，并没有使用 **插槽**(`<slot>`)。当使用插槽后：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <my-component>
            Can u see me ?
        </my-component>
    </div>
</body>
<script>
    let MyComponent = {
        template: `
            <div>
                My custom component
                <slot></slot>
            </div>
        `
    } 
    const app = new Vue({
        el: '#app',
        components: {
            MyComponent
        }
    })
</script>
</html>
```
浏览器上是这样的：
![](http://404j.fun/post-images/1589958807406.png)

总结：
尽量不要使用 DOM template[Why You Should Avoid Vue.js DOM Templates](https://vuejsdevelopers.com/2017/09/17/vue-js-avoid-dom-templates/), 如果非要，那么自定义组件不建议写成自闭和形式，因为 `Markup !== DOM` 标签不等于DOM，Vue模板引擎所到的并不一定是你所期望的结果。