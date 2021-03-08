---
layout: post
title: D3 中动态计算 x 轴 y 轴的宽度以及偏移量
categories: [d3, Blog]
description: 在 D3.js 中经常会遇到这样的情况：某一坐标轴不能正确展示的情况。
keywords: d3.js, emberjs , typescript , 坐标轴 , 调整坐标轴
---

在 D3.js 中经常会遇到这样的情况：某一坐标轴不能正确展示的情况。

如下图所示：
![2020-03-05-截屏2020-03-0513.23.23-83l8AA](https://raw.githubusercontent.com/FrankWang117/images/master/2020-03-05-截屏2020-03-0513.23.23-83l8AA.png)
造成这种情况的原因就是 y 轴上的数据过大，导致我们预留给 y 轴与左边界的空间不足以展示 y 轴上所有的文字。  
所以我们需要动态的计算 y 轴的宽度，通过 `transform` 属性，将 y 轴上的文字完整的显示出来。

## 原始代码

先上刚开始的代码：

```typescript
// some code
initLine() {
    const container = select(".bp-line")
    this.width = parseInt(container.style("width"))
    this.height = parseInt(container.style("height"))
    const padding = {
        top: 24,
        right: 24,
        bottom: 24,
        left: 24
    }
    const svg = container.append('svg')
        .attr("width", this.width)
        .attr('height', this.height)
        .style('background-color', "#fafbfc");

    const xScale = scaleBand()
        .domain(this.args.data.map((ele: any[]) => ele[0]))
        .range([padding.left, this.width - padding.right ])

    const xAxis = axisBottom(xScale)

    const yScale = scaleLinear()
        .domain([0, max(this.args.data.map((ele: any[]) => ele[1]))])
        .range([this.height - padding.top - padding.bottom, 0]);

    const yAxis = axisLeft(yScale)

    svg.append('g')
        .classed('x-axis', true)
        .attr("transform", `translate(0,${this.height - padding.bottom})`)
        .call(xAxis);

    svg.append('g')
        .classed('y-axis', true)
        .call(yAxis)

}
// some code
```

通过代码可以看出来，我们可以通过修改 y 轴的 `transform` 属性，使其移动到正确的位置上，那么这个属性的值需要设置为多少了？

## 移动 y 轴

这就需要我们进行计算，即坐标轴距离容器左边框有固定的 padding.left 同时再有坐标轴的宽度的距离，就是一个非常合适的位置了。  
这就需要我们去获取 y 轴的宽度：
通过

```typescript
// ...
// 动态获取y坐标轴的宽度
const yAxisWidth: number = svg.select(".y-axis").node().getBBox().width;

svg
  .select(".y-axis")
  .attr("transform", `translate(${padding.left + yAxisWidth},${padding.top})`);
// ...
```

此时我们可以看到 y 轴已经移到了合适的位置：
![2020-03-05-截屏2020-03-0513.24.32-8f0ac2](https://raw.githubusercontent.com/FrankWang117/images/master/2020-03-05-截屏2020-03-0513.24.32-8f0ac2.png)  
但是 x 轴的范围以及起始点有问题。我们这时候需要调整 x 轴的生成顺序，放在计算完 y 轴的宽度之后。

## 移动 x 轴

最后的代码是：

```typescript
// some code
initLine() {
    const container = select(".bp-line")
    this.width = parseInt(container.style("width"))
    this.height = parseInt(container.style("height"))
    const padding = {
        top: 24,
        right: 24,
        bottom: 24,
        left: 24
    }
    const svg = container.append('svg')
        .attr("width", this.width)
        .attr('height', this.height)
        .style('background-color', "#fafbfc");

    const yScale = scaleLinear()
        .domain([0, max(this.args.data.map((ele: any[]) => ele[1]))])
        .range([this.height - padding.top - padding.bottom, 0]);

    const yAxis = axisLeft(yScale)

    svg.append('g')
        .classed('y-axis', true)
        .call(yAxis)

    // 动态获取y坐标轴的宽度
    const yAxisWidth: number = svg.select('.y-axis').node().getBBox().width;

    svg.select(".y-axis")
        .attr("transform", `translate(${padding.left + yAxisWidth},${padding.top})`)


    // 最后绘制 x 坐标轴，可以根据y轴的宽度动态计算 x轴所占的宽度
    const xScale = scaleBand()
        .domain(this.args.data.map((ele: any[]) => ele[0]))
        .range([padding.left, this.width - padding.right - yAxisWidth])

    const xAxis = axisBottom(xScale)
    svg.append('g')
        .classed('x-axis', true)
        .attr("transform", `translate(${yAxisWidth},${this.height - padding.bottom})`)
        .call(xAxis);
}
// some code
```

## 结果

最后的结果可以看到 x 轴 y 轴都来到了合适的位置。  
![2020-03-05-截屏2020-03-0513.27.29-9dGNCe](https://raw.githubusercontent.com/FrankWang117/images/master/2020-03-05-截屏2020-03-0513.27.29-9dGNCe.png)
