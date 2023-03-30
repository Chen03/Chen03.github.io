---
title: "Worker.js"
slug: workerjs
date: 2023-03-30T10:28:18+08:00
comments: true
categories: development
---

Web Workers 可以让代码独立于主线程运行，避免大量的运算阻塞浏览器渲染画面，并且可以在计算过程中交互。  

## 如何使用

```js
// <script>
let worker = Worker("/path/to/worker.js")

worker.onmessage = event => {
  console.log(event.data)
}

worker.postMessage("Hello!")

//worker.js
onmessage = event => {
  if (event.data == "Hello!") {
    postMessage("How are you?")
  }
}
```

## 可以做到什么？

- 执行大型计算时，可以避免阻塞主线程  
- 好像没有了……  

但是真的在有用的时候很有用……  

## 关于 Service Worker

*内容来自 MDN*

Service Worker 是另一种 Worker，通常用于 Web App 中，可以截获网页的网络请求事件并对请求加以修改，从而实现一些很厉害的功能：

- 实现页面离线加载（使用缓存）
- 集中接收计算成本高的数据更新，比如地理位置和陀螺仪信息，这样多个页面就可以利用同一组数据
- 性能增强，比如预取用户可能需要的资源，比如相册中的后面数张图片
- 还有很多！

通过 Service Worker，可以实现离线的 Web App！