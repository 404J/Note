---
title: 'JavaScript in browser'
date: 2020-04-10 09:19:07
tags: [Browser,JavaScript]
published: true
hideInList: false
feature: http://404j-images.test.upcdn.net/coverImage/js_in_bowser.png
isTop: false
---
首先，浏览器是多进程工作的，每打开一个 tap 页面，就会新开一个独立的浏览器进程。
### 浏览器有哪些主要的进程
1. Browser进程：浏览器的主进程（负责协调、主控）
2. 第三方插件进程：每种类型的插件对应一个进程，仅当使用该插件时才创建
3. GPU进程：最多一个，用于3D绘制等
4. 浏览器渲染进程（浏览器内核）：**Renderer进程**，内部是多线程的，负责页面渲染，脚本执行，事件处理等
---
### 关于Renderer进程 💥
1. GUI渲染线程
   - 负责渲染浏览器界面，解析HTML，CSS，构建DOM树和RenderObject树，布局和绘制等
   - 当界面需要重绘（Repaint）或由于某种操作引发回流(reflow)时，该线程就会执行
2. JS引擎线程 💥
   - 也称为JS内核，负责处理Javascript脚本程序（例如V8引擎）
   - 单线程运行js
   - **和GUI渲染线程互斥，不会同时运行，由于Js是可以操作DOM的，如果JS引擎线程和GUI渲染线程同时运行，这样就会产生冲突**
3. 事件触发线程
   - 当对应事件触发（不论是WebAPIs完成事件触发，还是页面交互事件触发）时，该线程会将事件对应的回调函数放入callback queue（任务队列）中，等待js引擎线程的处理
4. 定时触发线程
   - 对应于setTimeout，setInterval API，由该线程来计时，当计时结束，将事件对应的回调函数放入任务队列中
   - 当setTimeout的定时的时间小于4ms，一律按4ms来算
5. 异步http请求线程
   - 在XMLHttpRequest在连接后是通过浏览器新开一个线程请求
   - 将检测到状态变更时，如果设置有回调函数，异步http请求线程就产生状态变更事件，将这个回调再放入任务队列中。再由JavaScript引擎执行
![](http://404j.fun:402/post-images/1586516977648.jpg)
---
### js如何在浏览器中运行 💥
![](http://404j.fun:402/post-images/1586742611626.jpg)
1. JS引擎线程提供js的运行环境，此时会形成一个**执行栈（stack）**，当调用一个函数时，就把它推入运行时中的栈中
2. **执行栈**中有调用Web Apis接口（诸如发送 HTTP 请求、监听 DOM 事件、延迟执行 setTimeout 或 setInterval）的时候，需要提供一个回调（callback）函数，则 JS 将其控制权连同一个 callback 委派给 Web API 后移动到该函数中的下一行
3. 此时Web Apis中的事件触发线程/定时触发线程/异步http请求线程就会去监听这些callback何时触发，当满足触发条件时，就会将这个callback推入到callback queue（任务队列）中
4. **事件循环（Event Loop）** 唯一的工作就是盯着 -- 回调队列上一有待执行（pending）的 callback 函数，就将其推入执行栈中；而这一动作发生的时间点，是 **栈一旦为空的时候**
5. 之后此callback就会被执行
---
### macrotask与microtask 

---
参考文章：
- [从浏览器多进程到JS单线程，JS运行机制最全面的一次梳理](https://juejin.im/post/5a6547d0f265da3e283a1df7#heading-0)
- [JS在浏览器和Node下是如何工作的？](https://blog.csdn.net/tonylua/article/details/103286267)
- [JavaScript 运行机制--Event Loop详解](https://segmentfault.com/a/1190000013823623)
- [JavaScript 运行机制详解：再谈Event Loop](https://www.ruanyifeng.com/blog/2014/10/event-loop.html)
- [Tasks, microtasks, queues and schedules](https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/)
- [process.nextTick()与promise.then()](https://segmentfault.com/q/1010000011914016)
