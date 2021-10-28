---
layout: post
title: 防抖和节流的实现
categories: Interview
description: 防抖和节流的实现以及展示
keywords: interview,debounce,throttle,防抖,节流
---

## 防抖（debounce）

指在事件被触发一定时间后再执行回调函数，如果在一定时间内该事件又被重复触发，则重启计时。

```javascript
function debounce(fn,delay) {
    let timer = null;
    return function(...args) {
        if(timer) {
            clearTimeout(timer)
        }
        timer = setTimeout(()=> {
            fn.apply(this,args)
        }),delay)
    }
}
```

其中，使用 `apply` 来改正 `this` 的指向，使其指定为正确的值。  
这样，一个基本的防抖函数就可以了。  
但是如果有新的需求：希望立即触发函数，然后等到停止触发 n 秒后，才可以重新触发执行。

```javascript
function debounce(fn, delay, isImmediate) {
  let timer = null;
  return function () {
    const context = this;
    if (timer) {
      clearTimeout(timer);
    }
    if (isImmediate) {
      let callNow = !timer;
      timers = setTimeout(() => {
        timer = null;
      }, delay);
      if (callNow) {
        fn.apply(context, arguments);
      }
    } else {
      timer = setTimeout(() => {
        fn.apply(this, arguments);
      }, delay);
    }
  };
}
```

## 节流（throttle）

如果持续触发事件，每隔一段时间，只执行一次事件。

```javascript
function throttle(fn, delay) {
  let startTime = 0;

  return function (...args) {
    let lastTime = Date.now();

    if (lastTime - startTime > delay) {
      fn.apply(this, args);
      startTime = Date.now();
    }
  };
}
```

使用时间戳实现，当事件触发时，立即执行一次。停止触发后没有办法再执行事件。
还有另外一种方式，是通过定时器实现：

```javascript
function throttle(fn, delay) {
  let timer;
  let previous = 0;
  return function (...args) {
    const context = this;
    if (!timer) {
      timer = setTimeout(function () {
        timer = null;
        fn.apply(context, args);
      }, delay);
    }
  };
}
```

使用定时器实现，会在时间触发 n 秒后第一次执行。停止触发后依然会在执行一次事件。
