> Promise 的实现

## 前言

ES6 使用 [Promise/A+](https://promisesaplus.com/) 规范。

## 实现 Promise

### 规范解读

Promise/A+ 规范主要分为术语、要求和注意事项三个部分，我们主要看一下第二部分也就是**要求部分**（Requirements），更详细的细节参照完整版 Promise/A+ 规范。

#### 1. Promise 状态

Promise 有三种状态：`pending`,`fulfilled`以及`rejected`.(`fulfilled` 以及 `resolved` 的[区别查看](https://segmentfault.com/q/1010000020423077/a-1020000020423898))。  

​	状态转换只能是 `pending` 到 `fulfilled` 或者 `pending` 到 `rejected`；

​	状态一旦转换完成，不能再次转换。

#### 2. `then` 方法

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

#### 3. Promise 解决过程

函数标识为 `[[Resolve]](promise,x)`,promise 为要返回的新promise 对象，x为 onFulfilled/onRejected 的返回值。如果 x 有 then 方法且看上去像一个 promise，我们就把 x 当成一个 promise 对象，即 thenable 对象，这种情况下尝试让 promise 接收 x 的状态。如果 x 不是 thenable 对象，就用 x 的值来执行 promise。

 `[[Resolve]](promise,x)` 函数具体运行规则：

- 如果 promise 和 x 指向同一对象，以 `TypeError` 为拒因（reason）拒绝执行 promise；
- 如果 x 为 promise，则使 promise 接受 x的状态；
  - 如果 x 为 pending 状态，promise 必须保持 pending 状态直到 x 变更为 fulfilled 或者 rejected
  - 如果 x 为 fulfilled 状态，以相同的值 fulfill promise
  - 如果 x 为 rejected 状态，以相同的原因（reason），reject promise
- 如果 x 为对象或者函数，取 x.then 的值，如果取值时出现错误，则让 promise 进入 rejected 状态。
  - 如果 then 不是函数，说明 x 不是 thenable 对象，直接以 x 的值 fulfill；
  - 如果 then 存在并且为函数，则把 x 作为 then 函数的作用域 this 调用。then 方法接收两个参数，第一个为 resolvePromise 以及第二个 rejectPromise；
    - 如果 resolvePromise 被执行，则以 resolvePromise 的参数 value 作为 x 继续调用 `[[Resolve]](promise,value)`,直到 x 不是对象或者函数；
    - 如果 rejectedPromise 被执行则让 promise 进入 rejected 状态；
- 如果 x 不是对象或者函数，直接就用 x 的值来执行（fulfill） promise

### 代码实现

规范解读第一条的代码实现：

```typescript
class Promise {
  // 定义 Promise 初始状态，为 pending
  status: string = 'pending';
	// 状态转换时携带的值，因为在 then 方法中需要处理 Promise成功或失败时的值
  // 所以需要一个全局变量存储
	data = '';
  
  // Promise 构造函数，传入参数为一个可执行的函数
	constructor(executor) {
    // resolve函数负责把状态转换为 resolved
    function resolve(value) {
      this.status = 'resolved';
      this.data = value
    }
    // reject 函数负责把状态转换为 rejected
    function reject(reason) {
      this.status = 'rejected';
      this.data = reason
    }
    // 直接执行executor函数，参数为处理函数resolve, reject。因为executor执行过程有可能会出错，错误情况需要执行reject
    try {
      executor(resolve,reject);
    } catch (e) {
      reject(e)
    }
  }
}
```

规范解读第二条的代码实现:

```javascript

then(onResolved,onRejected) {
  _onResolved = typeof onResolved === 'function' ? onResolved : function(v) {return v};
  _onRejected = typeof onRejected === 'function' ? onRejected : function(e) {throw e};
  
  let promise2;
  promise2 = new Promise((resolve,reject)=> {
    if(this.status === 'resolved') {
      try {
        const x = _onResolved(this.data);
        
        // 执行 [[Resolve]](promise2,x);
        resolvePromise(promise2,x,resolve,reject)
      } catch(e) {
        reject(e)
      }
    }
    if(this.status === 'rejected') {
      try {
        const x = _onRejected(this.data);
        resolvePromise(promise2,x,resolve,reject);
      } catch (e) {
        reject(e)
      }
    }
  })
  return promise2;
}
```

