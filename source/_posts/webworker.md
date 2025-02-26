---
title: webworker
date: 2025-02-26 15:22:51
tags:
---

# webworker实战场景

## 引言
> js是单线程的，在主线程中执行js代码，会阻塞页面的渲染和其他事件的处理，但是当遇到大量CPU密集型的计算时，会导致页面卡顿，影响用户体验。

## 为什么使用webworker
> webworker为js创建多线程环境,主线程开辟一个子线程，将计算的任务放在子线程中执行，计算完成后交回主线程，而不造成页面的卡顿。

## web worker 分类

- DedicatedWorker
    - 只能为一个页面使用(主要讲解)
- SharedWorker
    - 可以被多同源个页面使用

## web worker限制

### 1. 同源限制


### 2. 文件限制


### 3.DOM限制



### 4.模板引入问题

## 简单示例

```js
// App.vue
const worker = new Worker('http://localhost:5173/worker.js')


worker.addEventListener('message', (event) => {
  console.log("主线程收到woker线程的消息",event.data)
})

// 发送消息给worker线程
worker.postMessage('hello worker')
```

```js
// piblic/worker.js
self.addEventListener('message', function(e) {
    
    if(e.data){
        console.log("收到",e.data);
    }    
    // 发送消息给主线程
    self.postMessage(5000);
});

````


## 基本API



## 基本案例



