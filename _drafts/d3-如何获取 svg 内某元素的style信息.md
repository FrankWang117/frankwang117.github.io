对于 SVG 元素：  

使用
``` javascript
selection.node().getBBox()
//返回的值为：
{
    height: 5, 
    width: 5, 
    y: 50, 
    x: 20
} 
```

对于 HTML 元素
Use selection.node().getBoundingClientRect()

使用 
``` javascript
selection.node().getBoundingClientRect()
```
