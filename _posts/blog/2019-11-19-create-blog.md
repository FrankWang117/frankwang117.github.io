---
layout: post
title: 创建基于 jekyll 的 blog 记录
categories: Blog
description: 将创建 blog 的过程记录下来
keywords: blog,jekyll,创建博客,个人博客,GitHub pages
---

本文是使用 jekyll 做为博客蓝本来开发自己的博客的一个主要流程的回顾与总结。  

## 选择博客蓝本  
选择 jekyll 或其他。  

## 改变主题
将主题改为 `某某` 的并做自定义话设置。  

## 配置评论
我们需要为博客中的文章配置评论功能，这个时候使用 `Gitment` 或者 `Gitalk` 就是比较方便的做法了。  
因为上文提到的两个评论系统都是基于 GitHub 的 issus。支持在前端直接引入，不需要任何后端代码。可以在页面进行登录、查看、评论、点赞等操作，同时有完整的 Markdown / GFM 和代码高亮支持。尤为适合各种基于 GitHub Pages 的静态博客或项目页面。  
下面给出的链接是两者的官方 demo：  
    - Gitment demo ： [demo](https://imsun.github.io/gitment/)
    - Gitalk demo: [demo](https://gitalk.github.io/)  

现在来看看如何给自己的 jekyll 博客添加相应的评论功能吧（以 Gitalk 为例）.  
### 1. 创建 Github application
在如图所示的 `Settings` 链接中：
<div align="center">
    <img alt="设置第一步" src="https://raw.githubusercontent.com/FrankWang1991/images/master/github-setting.png">
</div>
点击下图所示区域的左下方的 `Developer settings`：  
<div align="center">
    <img alt="开发者设置" src="https://raw.githubusercontent.com/FrankWang1991/images/master/D3fiyl.png">
</div>
## 可能遇到的错误
在部署博客的时候，某天打开发现页面的结构发生混乱，打开控制台才发现一些静态资源请求失败，比如：  
<div align="center">
    <img alt="静态文件缺失" src="https://raw.githubusercontent.com/FrankWang1991/images/master/0RTV2u.png">
</div>
``` 
GET https://frankwang1991.github.io/assets/vendor/primer-css/css/primer.css net::ERR_ABORTED 404
``` 
而还有一些同样的静态资源却加载成功了，仔细查看了下，加载失败的静态文件目录全部是在项目的 `assets/vendor` 下面，然后查看了项目的目录，发现并没有此目录文件夹。  
打开本地的代码库，发现此目录安安静静的躺着这里：  
<div align="center">
    <img alt="vendor 本地目录" src="https://raw.githubusercontent.com/FrankWang1991/images/master/SmoABz.png">
</div>
那这问题很明显，找到了 `.gitignore` 文件：  
<div align="center">
    <img alt=".gitignore 文件内容" src="https://raw.githubusercontent.com/FrankWang1991/images/master/YFYnPh.png">
</div>
将其中的 `vendor` 去掉即可。（CNAME 为自定义域名需要使用的，这里由于域名备案还未完成，先将 CNAME 文件放入此中来）。现在 `.gitignore` 应该是这个样子：

``` 
_site
.sass-cache
.jekyll-cache
.jekyll-metadata
CNAME
``` 

