---
layout: post
title: 浏览器的事件循环
categories: Blog
description: 浏览器中的事件循环以及 Nodejs 中的事件循环异同。
keywords: event loop,event loops,Node.js,事件循环
---

## 概览

![图片](https://raw.githubusercontent.com/FrankWang117/images/master/2021-04-11/640.png?token=AFFPCSH4GU44TB5CVCQ6I6DAOKDAK)

## 为什么

JavaScript 是单线程，也就是同一时间只能做一件事情。

> 设计成单线程的原因是：
>
> 假定 JavaScript 同时有两个线程，一个线程在某个 DOM 节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为主就成了问题。

JavaScript 从诞生之初就是单线程，但是单线程就导致有很多任务需要排队执行，只有一个任务执行完才能执行后一个任务。如果某个任务执行时间太长，就容易造成阻塞；为了解决这一问题，JavaScript 引入了**事件循环机制**。

## 是什么

JavaScript 单线程任务被分为**同步任务**和**异步任务**；

- 同步任务：立即执行的任务，在主线程上排队执行，前一个任务执行完毕，才能执行后一个任务；
- 异步任务：异步执行的任务，不进入主线程，而是在异步任务有了结果之后，将注册的回调函数放入任务队列中等待主线程空闲的时候读取执行；

> 注意：异步函数在相应辅助线程中处理完成后，即异步函数达到了触发条件了，就把回调函数推入任务队列中，而不是说注册一个异步任务就会被放入这个任务队列中

同步任务与异步任务流程图比较：

![图片](https://raw.githubusercontent.com/FrankWang117/images/master/2021-04-11/640-20210411210343685.png?token=AFFPCSA4IP6PI3RBPXANUCDAOLZ62)

从上面的流程图中可以看到，主线程不断从任务队列中读取事件，这个过程是循环不断的，这种运行机制就叫做 Event Loop（事件循环）。

## 事件循环中的两种任务

在 JavaScript 中，除了广义的同步任务和异步任务，还可以细分，一种是宏任务（`MacroTask`）也叫 Task，一种叫微任务（`MicroTask`）。

二者执行顺序流程图如下：

![图片](https://raw.githubusercontent.com/FrankWang117/images/master/2021-04-11/640-20210411210652254.png?token=AFFPCSGXQQPLBSXRSN3353TAOL2KS)

每次单个**宏任务**执行完毕后， 检查**微任务队列**是否为空， 如果不为空，会按照**先入先出**的规则执行完全部的**微任务**后， 清空微任务队列， 然后再执行下一个**宏任务**，如此循环。

如何区分宏任务与微任务呢？

- 宏任务(macroTask)，又称 task。可以理解为每次**执行栈**执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。包括
  - script
  - setTimeout
  - setInterval
  - setInmediate
  - I/O 操作
- 微任务(microTask)，又称 job，可以理解是在当前 task 执行结束后立即执行的任务。包括：
  - `Promise.then/cath /finally`回调（平时常见的）
  - MutationObserver 回调（html5 新特性）

## 为什么要有微任务呢？

既然我们知道了微任务与宏任务，但异步任务为什么要区分宏任务与微任务呢，只有宏任务不可以吗？

因为事件队列其实是一个“先进先出”的数据结构，排在前面的事件会优先被主线程读取， 那如果突然来了一个优先级更高的任务，还让去人家排队，就很不理性化， 所以需要引入微任务。

举一个现实生活中的例子：

> 就是我们去银行办理业务时， 并不是到了就能办理， 而是需要先取号排队， 等到柜台业务员办理完当前客户业务才能继续叫号进行下一个。
>
> 这时每一个来办理业务的人就可以认为是银行柜员的一个宏任务来存在， 当办理到你的业务时， 你本来只是要重新绑定一下手机号， 但是突然想到明天要参加婚礼，需要随份子钱， 此时你和柜员说你要取 money, 这时候柜员不能告诉你，让你重新取号排队（不合理的要求）。
>
> 其实这时候就相当于你突然提出了一个新的任务，这个任务就相当于是一个`微任务` ,它要在下一个宏任务之前完成。

**在当前的微任务没有执行完成时，是不会执行下一个宏任务的。**

## 事件循环典型题目分析

### 题目一

```javascript
// 代码执行结果是什么
async function async1() {
  console.log("async1 start");
  await async2();
  console.log("async1 end");
}

async function async2() {
  console.log("async2");
}

console.log("script start");

setTimeout(function () {
  console.log("setTimeout");
}, 0);

async1();

new Promise(function (resolve) {
  console.log("promise1");
  resolve();
}).then(function () {
  console.log("promise2");
});

console.log("script end");
```

错误结果

```markdown
- script start
- promise1
- script end
- promise2
- setTimeout
- async1 start
- async2
- async1 end
```

async(es8) 的定义见[此处](https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous/Async_await)

```
<details>
  <summary>什么是iuap design</summary>
  iuap design 是用友网络FED团队开发的企业级应用前端集成解决方案。
</details>
```

<details>
  <summary>正确结果</summary>
  <pre><code>
    script start
    async1 start
    async2
    promise1
    script end
    async1 end
    promise2
    setTimeout
    </code></pre>
</details>

<details>
	<summary>结果分析</summary>
  <ul style="list-style-type: decimal">
    <li>建立执行上下文，先执行同步任务，输出『script start』</li>
    <li>往下执行，遇到<code>setTimeout</code>，将其回调函数放入宏任务队列，等待执行</li>
    <li>继续往下执行，调用`async1`:</li>
    <li>继续执行，有个new Promise 输出『promise1』,当`resolve`后，将`.then()`的回调函数放到微任务队列中（记住Promise本身是同步的立即执行函数，then是异步执行函数）。</li>
    <li>继续往下执行， 输出『script end』，此时调用栈被清空，可以执行异步任务</li>
    <li>开始第一次事件循环：
    	<ul>
        <li>由于整个script 算一个宏任务，因此该宏任务已经执行完毕</li>
        <li>检查微任务队列， 发现其中放入了2个微任务（分别在3.2步，4步放入）， 执行输出『async1 end』，『promise2』，第一次循环结束</li>
      </ul>
    </li>
    <li>开始第二次循环：
    	<ul>
      	<li>从宏任务开始， 检查宏任务队列中有<code>setTimeout</code>回调， 输出『setTimeout』</li>
      	<li>检查微任务队列，无可执行的微任务， 第二次循环结束</li>
      </ul>
    </li>
  </ul>
</details>

> 注意：async/await 底层是基于 Promise 封装的，所以 await 前面的代码相当于 new Promise，是同步进行的，await 后面的代码相当于.then 回调，才是异步进行的。

关于第 3 步代码执行分析：

```
async function async1() {
    console.log("async1 start")
    await async2()
    console.log("async1 end")
}
```

改为 Promise 写法就是：

```
async function async1() {
new Promise((resolve, reject) =>{
    console.log("async1 start")
    resolve(async2())
}).then(()=>{
    // 执行 async1 函数await之后的语句
    console.log("async1 end")
})
}
```

题目二：

```javascript
console.log("start");
setTimeout(() => {
  console.log("children2");
  Promise.resolve().then(() => {
    console.log("children3");
  });
}, 0);

new Promise(function (resolve, reject) {
  console.log("children4");
  setTimeout(function () {
    console.log("children5");
    resolve("children6");
  }, 0);
}).then((res) => {
  console.log("children7");
  setTimeout(() => {
    console.log(res);
  }, 0);
});
```

结果：

```
start;
children4;
children2;
children3;
children5;
children7;
children6;
```

## Node.js 和浏览器的事件循环的区别

Node.js 的事件循环是 `libuv` 实现的，官网图示：

![图片](https://raw.githubusercontent.com/FrankWang117/images/master/2021-04-13/640.png?token=AFFPCSB6XWLZOLMMH7AOTEDAOTYOS)

图中表示的是事件循环包含的 6 个阶段，大体的 task（宏任务）执行顺序是这样的：

- timers 定时器：本阶段执行已经安排的 `setTimeout()` 和 `setInterval()` 的回调函数。
- pending callbacks 待定回调：执行延迟到下一个循环迭代的 I/O 回调。
- idle, prepare：仅系统内部使用,可以不必理会。
- poll 轮询：检索新的 `I/O` 事件;执行与 `I/O` 相关的回调（几乎所有情况下，除了关闭的回调函数，它们由计时器和 `setImmediate()` 排定的之外），其余情况 node 将在此处阻塞。
- check 检测：`setImmediate()` 回调函数在这里执行。
- close callbacks 关闭的回调函数：一些准备关闭的回调函数，如：`socket.on('close', ...)`。

首先需要知道的是 Node 版本不同，执行顺序有所差异。因为`Node v11`之后， 事件循环的原理发生了变化，**和浏览器执行顺序趋于一致**，都是每执行一个宏任务就执行完微任务队列。

在`Node v10`及以前，微任务和宏任务在 Node 的执行顺序：

1. 执行完一个阶段的所有任务
2. 执行完`nextTick`队列里面的内容
3. 然后执行完微任务队列的内容

在`Node v10`及以前的版本，微任务会在事件循环的各个阶段之间执行，也就是一个阶段执行完毕，就会去执行微任务队列的任务：

![图片](https://raw.githubusercontent.com/FrankWang117/images/master/2021-04-13/640-20210413091738001.png?token=AFFPCSBL6ZLV7PZOQI66ID3AOTYW6)

## 浏览器和 Node.js 执行顺序分别是什么？

```javascript
setTimeout(() => {
  console.log("timer1");

  Promise.resolve().then(function () {
    console.log("promise1");
  });
}, 0);

setTimeout(() => {
  console.log("timer2");

  Promise.resolve().then(function () {
    console.log("promise2");
  });
}, 0);

// 浏览器中：
timer1;
promise1;
timer2;
promise2;

// 在Node中：
timer1;
timer2;
promise1;
promise2;
```

在这个例子中，Node 的逻辑如下（再强调一下 Node v10 及以下）：

最初 timer1 和 timer2 就在 timers 阶段中。开始时首先进入 timers 阶段，执行 timer1 的回调函数，打印 timer1，并将 promise1.then 回调放入微任务队列，同样的步骤执行 timer2，打印 timer2；至此，timer 阶段执行结束，event loop 进入下一个阶段之前，执行微任务队列的所有任务，依次打印 promise1、promise2。

### setImmediate 的 setTimeout 的区别

`setImmediate`大部分浏览器暂时不支持，只有 IE10、11 支持，具体可见[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setImmediate) 。`setImmediate`和`setTimeout`是相似的，但根据它们被调用的时间以不同的方式表现。

- `setImmediate`设计用于在当前 poll 阶段完成后 check 阶段执行脚本 。
- `setTimeout` 安排在经过最小（ms）后运行的脚本，在 timers 阶段执行。

```javascript
setTimeout(() => {
  console.log("setTimeout");
}, 0);
setImmediate(() => {
  console.log("setImmediate");
});
```

其执行顺序为：

遇到`setTimeout`，虽然设置的是 0 毫秒触发，但是被 node.js 强制改为 1 毫秒，塞入 times 阶段 遇到`setImmediate`塞入 check 阶段 同步代码执行完毕，进入 Event Loop 先进入 times 阶段，检查当前时间过去了 1 毫秒没有，如果过了 1 毫秒，满足`setTimeout`条件，执行回调，如果没过 1 毫秒，跳过 跳过空的阶段，进入 check 阶段，执行`setImmediate`回调 可见，1 毫秒是个关键点，所以在上面的例子中，`setImmediate`不一定在`setTimeout`之前执行了。

### process.nextTick()与 Promise 回调谁先执行？

`process.nextTick()`是 Node 环境下的方法， 所以我们基于 Node 谈论。

`process.nextTick()`是一个特殊的异步 API，其不属于任何的 Event Loop 阶段。事实上 Node 在遇到这个 API 时，Event Loop 根本就不会继续进行，会马上停下来执行`process.nextTick()`，这个执行完后才会继续 Event Loop。可以看一下个例子：

```javascript
var fs = require('fs')

fs.readFile(__filename, () => {
    setTimeout(() => {
        console.log('setTimeout');
    }, 0);

    setImmediate(() => {
        console.log('setImmediate');

        process.nextTick(() => {
          console.log('nextTick 2');
        });
    });

    process.nextTick(() => {
      console.log('nextTick 1');
    });
});

// 执行结果
nextTick 1
setImmediate
nextTick 2
setTimeout
```

执行流程梳理：

1. 代码都在`readFile`回调中，回调执行时处于`poll`阶段
2. 遇到`setTimeout`，虽然延时设置的是 0， 但是相当于`setTimeout(fn,1)`,将其回调函数放入后面的 timers 阶段
3. 接下来遇到`setImmediate`,将其回调函数放入到后面的 check 阶段
4. 遇到`process.nextTick`, 立即执行， 输出 『nextTick 1』
5. 执行到下一个阶段 check,输出『setImmediate』， 又遇到`nextTick`,执行输出『nextTick 2』
6. 执行到下一个 timers 阶段， 输出『setTimeout』

这种机制其实类似于我们前面讲的微任务，但是并不完全一样,比如同时有`nextTick`和`Promise`的时候，肯定是`nextTick`先执行，原因是`nextTick`的队列比`Promise`队列优先级更高。来看个例子:

```javascript
setImmediate(() => {
  console.log("setImmediate");
});
Promise.resolve().then(() => {
  console.log("promise");
});
process.nextTick(() => {
  console.log("nextTick");
});

// 运行结果
nextTick;
promise;
setImmediate;
```

## 总结

文章包含了为什么会有事件循环， 事件循环是什么，事件循环的运行机制以及 Node 和浏览器中事件循环的异同点，
