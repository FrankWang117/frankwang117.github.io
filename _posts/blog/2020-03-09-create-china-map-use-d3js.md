---
layout: post
title: 使用 D3.js 创建根据值域颜色渐变的地图
categories: [d3, Blog]
description: 使用 D3.js 结合地图 geojson 数据以及需要展示的数据，绘制带有值域渐变的中国地图
keywords: d3.js, emberjs , typescript , 地图 
---
  
在实际开发中,地图是个比较常见的图,用于展示各个省市之间的数据差异。只要有了所需绘制地图的 geojson 
文件我们就能通过此方法绘制我们需要的任何地图。  
 
项目地址: [点击查看](https://github.com/FrankWang1991/ember-d3-demo)
@[TOC]
## 效果预览
先来看一下完成后的效果图:
![china-map](https://raw.githubusercontent.com/FrankWang1991/images/master/2020-03-09-截屏2020-03-0914.17.36-0AElRN.png)

很清晰的展示了各个省份的数据之间的差异.同时还有 visualMap 来展示数据的范围.当然不能缺少的是中国的南海诸岛区域(未完成调试).  

获取DOM,向DOM 中插入 svg 就不在赘述,具体看最后的详细代码.这里从获取 geojson 数据开始.  
## 获取地图 geojson 数据

要想绘制地图,首先要有地图的 geojson 数据.在 D3 的 v5 版本中,使用 Promise 替代了之前版本中的回调方式:

```javascript
json('../json/chinawithoutsouthsea.json')
    .then(geoJson => {
         const projection = geoMercator()
                  .fitSize([layout.getWidth(), layout.getHeight()], geoJson);
              const path = geoPath().projection(projection);
    
              const paths = svg
                  .selectAll("path.map")
                  .data(geoJson.features)
                  .enter()
                  .append("path")
                  .classed("map",true)
                  .attr("fill", "#fafbfc")
                  .attr("stroke", "white")
                  .attr("class", "continent")
                  .attr("d", path)
                  .on('mouseover', function (d: any) {
                      select(this)
                          .classed('path-active', true)
                  })
                  .on('mouseout', function (d: any) {
                      select(this)
                          .classed('path-active', false)
                  })
              
                  const t = animationType();
                        // animationType = function() {
                  //       return d3.transtion().ease()
                  // }
    
              paths.transition(t)
                  .duration(1000)
                  .attr('fill', (d: any) => {
                      let prov = d.properties.name;
                      let curProvData = data.find((provData: any) => provData[0] === prov.slice(0, 2))
    
                      return color(curProvData ? curProvData[2] : 0)
                  });
          });
```

这段代码首先是获取一个地图的投影:

```javascript
const projection = geoMercator()
    .fitSize([layout.getWidth(), layout.getHeight()], geoJson);

const path = geoPath().projection(projection);
```

注意这里,使用的是 fitSize API,它比以往使用的获取投影之后,进行 translate 以及 scale 要方便的多,在之前的版本中,我们可能要写:

```javascript
        /**
         * old method 需要手动计算scale 以及 center
         const projection = geoMercator()
            .translate([layout.getWidth() / 2, layout.getHeight() / 2])
            .scale(860).center([107, 40]);
         */
```

现在使用的 fitSize 可以很好的将 geojson 的路径绘制在容器的中心.并自适应大小.当然,这种方法好用的前提是需要一个规范的 geojson 文件的支持.不然还是只能使用之前的 translate 并 scale 的方法.  
## 绘制 svg 元素
获取了数据之后,就是对其进行绘制,还是与之前绘制图表的方式差不多. **注意传入 data 方法的参数**;    

最后添加的动画是进行数据的映射对数据遍历,获取到数据中与路径中 name 属性相同的,进行颜色的填充.
## 南海诸岛的添加

由于一般的中国地图会将南海诸岛区域按照正常的方位展示,但是这样在数据展示图上会带来一定的不便以及占用一些空间.所以这次选择的是将南海诸岛以 svg 图的形式引入进来进行放置(注意比例尺-图中未严格按照比例尺进行缩放).这同样的需要 xml 请求:

```javascript
xml("../json/southchinasea.svg").then(xmlDocument => {
            svg.html(function () {
                return select(this).html() + xmlDocument.getElementsByTagName("g")[0].outerHTML;
            });
            const southSea = select("#southsea")

            let southSeaWidth = southSea.node().getBBox().width / 5
            let southSeaH = southSea.node().getBBox().height / 5
            select("#southsea")
                .classed("southsea", true)
                .attr("transform", `translate(${layout.getWidth()-southSeaWidth-24},${layout.getHeight()-southSeaH-24}) scale(0.2)`)
                .attr("")
        })
```


没啥说的.  

## visualMap 的添加
最后就是 visualMap 的添加,让数据展示更加具体.

```javascript
// 显示渐变矩形条
        const linearGradient = svg.append("defs")
            .append("linearGradient")
            .attr("id", "linearColor")
            //颜色渐变方向
            .attr("x1", "0%")
            .attr("y1", "100%")
            .attr("x2", "0%")
            .attr("y2", "0%");
        // //设置矩形条开始颜色
        linearGradient.append("stop")
            .attr("offset", "0%")
            .attr("stop-color", '#8ABCF4');
        // //设置结束颜色
        linearGradient.append("stop")
            .attr("offset", "100%")
            .attr("stop-color", '#18669A');

        svg.append("rect")
            //x,y 矩形的左上角坐标
            .attr("x", layout.getPadding().pl)
            .attr("y", layout.getHeight() - 83 - layout.getPadding().pb) // 83为矩形的高
            //矩形的宽高
            .attr("width", 16)
            .attr("height", 83)
            //引用上面的id 设置颜色
            .style("fill", "url(#" + linearGradient.attr("id") + ")");
        //设置文字

        // 数据初值
        svg.append("text")
            .attr("x", layout.getPadding().pl + 16 + 8)
            .attr("y", layout.getHeight() - layout.getPadding().pb)
            .text(0)
            .classed("linear-text", true);
        // visualMap title
        svg.append("text")
            .attr("x", layout.getPadding().pl)
            .attr("y", layout.getHeight() - 83 - layout.getPadding().pb - 8) // 8为padding
            .text('市场规模')
            .classed("linear-text", true);
        //数据末值
        svg.append("text")
            .attr("x", layout.getPadding().pl + 16 + 8)
            .attr("y", layout.getHeight() - 83 - layout.getPadding().pb + 12) // 12 为字体大小
            .text(format("~s")(maxData))
            .classed("linear-text", true)
```

也是根据 svg 中的一些元素来形成 visualMap 图.  


## 完成代码
最后完整的代码是

```javascript
import Component from '@glimmer/component';
import { action } from '@ember/object';
import { json, xml } from 'd3-fetch';
import { scaleLinear } from 'd3-scale'
import Layout from 'ember-d3-demo/utils/d3/layout';
import { geoPath, geoMercator } from 'd3-geo';
import { max, min } from 'd3-array';
import { select } from 'd3-selection';
import { format } from 'd3-format';
import {animationType} from '../../../../utils/d3/animation';

interface D3BpMapArgs {
    data: any[]
    // [
    //     ["广东", 1, 73016024],
    //     ["河南", 1, 60152736],
    //     ...
    // ]
    width: number
    height: number
}

export default class D3BpMap extends Component<D3BpMapArgs> {
    @action
    initMap() {
        let layout = new Layout('.bp-map')
        let { width, height, data } = this.args
        if (width) {
            layout.setWidth(width)
        }
        if (height) {
            layout.setHeight(height)
        }
        const container = layout.getContainer()

        //generate svg
        const svg = container.append('svg')
            .attr('width', layout.getWidth())
            .attr('height', layout.getHeight())
            .style('background-color', '#FAFBFC');

        /**
         * old method 需要手动计算scale 以及 center
         const projection = geoMercator()
            .translate([layout.getWidth() / 2, layout.getHeight() / 2])
            .scale(860).center([107, 40]);
         */
        const maxData = max(data.map((datum: any[]) => datum[2]))
        const minData = min(data.map((datum: any[]) => datum[2]))

        const color = scaleLinear().domain([0, maxData])
            .range(['#B8D4FA', '#18669A']);
        // .range(["#E7F0FE","#B8D4FA","#8ABCF4","#5CA6EF",
        //     "#3492E5",
        //     "#1E7EC8",
        //     "#18669A"
        // ])
        xml("../json/southchinasea.svg").then(xmlDocument => {
            svg.html(function () {
                return select(this).html() + xmlDocument.getElementsByTagName("g")[0].outerHTML;
            });
            const southSea = select("#southsea")

            let southSeaWidth = southSea.node().getBBox().width / 5
            let southSeaH = southSea.node().getBBox().height / 5
            select("#southsea")
                .classed("southsea", true)
                .attr("transform", `translate(${layout.getWidth()-southSeaWidth-24},${layout.getHeight()-southSeaH-24}) scale(0.2)`)
                .attr("")
             return json('../json/chinawithoutsouthsea.json')
        })
            .then(geoJson => {
                const projection = geoMercator()
                    .fitSize([layout.getWidth(), layout.getHeight()], geoJson);
                const path = geoPath().projection(projection);

                const paths = svg
                    .selectAll("path.map")
                    .data(geoJson.features)
                    .enter()
                    .append("path")
                    .classed("map",true)
                    .attr("fill", "#fafbfc")
                    .attr("stroke", "white")
                    .attr("class", "continent")
                    .attr("d", path)
                    .on('mouseover', function (d: any) {
                        select(this)
                            .classed('path-active', true)
                    })
                    .on('mouseout', function (d: any) {
                        select(this)
                            .classed('path-active', false)
                    })
                
                    const t = animationType();

                paths.transition(t)
                    .duration(1000)
                    .attr('fill', (d: any) => {
                        let prov = d.properties.name;
                        let curProvData = data.find((provData: any) => provData[0] === prov.slice(0, 2))

                        return color(curProvData ? curProvData[2] : 0)
                    });
            //     return xml("../json/southchinasea.svg")
            });
        // 显示渐变矩形条
        const linearGradient = svg.append("defs")
            .append("linearGradient")
            .attr("id", "linearColor")
            //颜色渐变方向
            .attr("x1", "0%")
            .attr("y1", "100%")
            .attr("x2", "0%")
            .attr("y2", "0%");
        // //设置矩形条开始颜色
        linearGradient.append("stop")
            .attr("offset", "0%")
            .attr("stop-color", '#8ABCF4');
        // //设置结束颜色
        linearGradient.append("stop")
            .attr("offset", "100%")
            .attr("stop-color", '#18669A');

        svg.append("rect")
            //x,y 矩形的左上角坐标
            .attr("x", layout.getPadding().pl)
            .attr("y", layout.getHeight() - 83 - layout.getPadding().pb) // 83为矩形的高
            //矩形的宽高
            .attr("width", 16)
            .attr("height", 83)
            //引用上面的id 设置颜色
            .style("fill", "url(#" + linearGradient.attr("id") + ")");
        //设置文字

        // 数据初值
        svg.append("text")
            .attr("x", layout.getPadding().pl + 16 + 8)
            .attr("y", layout.getHeight() - layout.getPadding().pb)
            .text(0)
            .classed("linear-text", true);
        // visualMap title
        svg.append("text")
            .attr("x", layout.getPadding().pl)
            .attr("y", layout.getHeight() - 83 - layout.getPadding().pb - 8) // 8为padding
            .text('市场规模')
            .classed("linear-text", true);
        //数据末值
        svg.append("text")
            .attr("x", layout.getPadding().pl + 16 + 8)
            .attr("y", layout.getHeight() - 83 - layout.getPadding().pb + 12) // 12 为字体大小
            .text(format("~s")(maxData))
            .classed("linear-text", true)
    }
}

```

