## 前言

ES6 使用 [Promise/A+](https://promisesaplus.com/) 规范。

## 实现 Promise

### 规范解读

Promise/A+ 规范主要分为术语、要求和注意事项三个部分，我们主要看一下第二部分也就是**要求部分**（Requirements），更详细的细节参照完整版 Promise/A+ 规范。

#### Promise 状态

Promise 有三种状态：`pending`,`fulfilled`以及`rejected`.(`fulfilled` 以及 `resolved` 的[区别查看](https://segmentfault.com/q/1010000020423077/a-1020000020423898))。  

​	状态转换只能是 `pending` 到 `fulfilled` 或者 `pending` 到 `rejected`；

​	状态一旦转换完成，不能再次转换。

#### `then` 方法

Promise 必须拥有一个 `then` 方法，来处理 fulfilled 或 rejected 状态下的值。

Promise 的 `then` 方法接受两个参数:

```javascript
promise.then(onFulfilled,onRejected)
```

- 两个参数均为可选参数，类型均为函数，如果不是将被忽略；

- 如果 `onFulfilled` 是个函数，则必须（must）在 promise 是 fulfilled 状态后调用，并将 promise 的值作为第一个参数。不能（must not be）在 promise 是 fulfilled 状态之前调用。只能执行一次。

- 同理，如果 onRejected 是个函数，则必须（must）在promise 是 rejected 状态后调用，并将 promise 的值作为第一个参数。不能（must not be）在 promise 是 rejected 状态之前调用。只能执行一次。

- `onFulfilled` 和 `onRejected` 只有在[执行环境](http://es5.github.io/#x10.3)堆栈仅包含**平台代码**时才可被调用

- `then` 方法必须返回一个新的 `promise`，记作 `promise2`。

  ```javascript
  promise2 = promise1.then(onFulfilled,onRejected)
  ```

  

  这也就保证了`then `方法可以在同一个 `promise` 上多次调用。（ps：规范只要求返回 `promise`，并没有明确要求返回一个新的 `promise`，这里为了跟ES6实现保持一致，我们也返回一个新 `promise`）;

- onFulfilled/onRejected 有返回值则把返回值定义为 x，并执行 `[[Resolve]](promise)` ;

- onFulfilled/onRejected 运行出错，则把 promise2 设置为 rejected 状态；

- onFulfilled/onRejected 不是函数，则需要把 promise1 的状态传递下去；

- 

