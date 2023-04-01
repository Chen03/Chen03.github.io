---
title: "Worker.js"
description: 冰岩分享：2分钟精通 Worker.js
date: 2023-03-30T10:28:18+08:00
image: cover.JPG
categories: dev
tags: 
  - JavaScript
---

Web Workers 可以让代码独立于主线程运行，避免大量的运算阻塞浏览器渲染画面，并且可以在计算过程中交互。  
其实它的功能和API非常简单，就是多线程工具。  

## 如何使用

### 网页端

1. 创建 Worker

```js
let worker = Worker("/path/to/worker.js")
```

2. 接收来自 Worker 的信息

```js
worker.onmessage = event => {
  console.log(event.data)
}
```

3. 向 Worker 发送信息

```js
worker.postMessage("Hello!")
```

### worker.js

1. 接受信息

```js
onmessage = event => {
  if (event.data == "Hello!") {
    //Do something...
  }
}
```

2. 发送信息给网页

```js
postMessage("How are you?")
```

### 结合起来...

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

## 一个例子

[算法大作业](https://chen03.github.io/Fifty-five-game/)

### calc.worker.js

```js
function calculate() {
  console.log('calculate');
  while (queue.size !== 0 && !equal(dest, queue.peek().stat)) {
    //算法过程...
    if (lim % 23 === 0) {
      postMessage({type: 'stat', stat: now.stat, step: lim}); //发送计算过程到网页
    }
  }

  //计算结束
  hasStopped = true;
  postMessage({type: 'stop', dist: queue.peek().step, time: ((new Date).getTime())});
  postMessage({type: 'stat', stat: dest, step: lim});
}

// 以下为与网页交互内容
onmessage = (ev) => {
  switch (ev.data.type) {
    case 'start':
      //开始算法过程
      break;
  }
}
```

### 网页端

```js
function App() {
  ...

  const [worker, setWorker] = useState(null);
  useEffect(() => {
    if (!worker) {
      var worker = new Worker(new URL('./calc.worker.js', import.meta.url));  //新建 Worker
      worker.onmessage = (ev) => {
        switch(ev.data.type) {
          case 'stat':
            startTransition(
              () => {
                setBoard(ev.data.stat);
                setStep(ev.data.step);
              });
            break;
          case 'stop':
            setStopped(true);
            setDist(ev.data.dist);
            break;
        }
      }
      setWorker(worker);
    }
  }, []);

  return ( ... );
}
```

## 可以做到什么？

- 执行大型计算时，可以避免阻塞主线程  
- 好像没有了……  

但是真的在有用的时候很有用……  

## TIPS

如果你在使用 [Webpack](https://webpack.js.org/guides/web-workers/) 或者 [Vite](https://vitejs.dev/guide/features.html#web-workers), 要这样创建 Worker:
```js
new Worker(new URL('./worker.js', import.meta.url));
```

## 关于 Service Worker

*内容来自 MDN*

Service Worker 是另一种 Worker，通常用于 Web App 中，可以截获网页的网络请求事件并对请求加以修改，从而实现一些很厉害的功能：

- 实现页面离线加载（使用缓存）
- 集中接收计算成本高的数据更新，比如地理位置和陀螺仪信息，这样多个页面就可以利用同一组数据
- 性能增强，比如预取用户可能需要的资源，比如相册中的后面数张图片
- 还有很多！

通过 Service Worker，可以实现离线的 Web App！