使用原生 JavaScript 实现 Ajax 的函数
## 实现结果
ajax 函数我们都会在日常的开发中用到，基本的使用方式是：
``` javascript
ajax({
	url: "/user",
	type: "POST",
	data: JSON.stringify({id:1}),
	dataType: "json",
	success: function(res,xml){ /* 请求成功后执行的代码 */},
	error: function(status){ /* 请求失败后执行的代码 */}
});
```
## 具体步骤
``` javascript
function ajax(options) {
	options = options || {};
	options.type = (options.type || "GET").toUpperCase();
	options.dataType = options.dataType || "json";
	var param = formatParams(options.data);
	
	// 第一步 创建 XHR 对象
	if(window.XMLHttpRequest) {
		// 非 IE6
		var xhr = new XMLHttpRequest();
	} else {
		// IE 6 及其以下版本浏览器
		var xhr = new ActiveXObject("Microsoft.XMLHTTP");
	}
	// 第二步
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMyNjEwMjEzNl19
-->