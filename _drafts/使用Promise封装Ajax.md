---
layout: post
title: 使用 Promise 封装原生 Http 请求
categories: [Javascript]
description: 使用 Promise 来封装原生的 Http 请求
keywords: JavaScript, promise, ajax,http
---

使用原生 http 请求来实现 Ajax 请求，应该都不会陌生。但是 Ajax 存在着“回调地狱”的问题，在 ES6
中，我们学到了 Promise 可以解决这种“回调地狱”的现象。那就使用 Promise 来实现 Http 请求吧。  

具体代码：  
``` javascript
const getJSON = function(url) {
    return new Promise((resolve,reject)=> {
        const handle = function() {
            if(this.readyState !== 4) {
                return;
            }
            if(this.status === 200) {
                resolve(this.response)
            } else {
                reject(new Error(this.statusText))
            }
        }
        const client = new XMLHttpRequest();
        client.open("GET",url);
        client.onreadystatechange = handle;
        client.responseType = "json";
        client.setRequestHeader("Accept","application/json");
        client.send();
    })
}
```
