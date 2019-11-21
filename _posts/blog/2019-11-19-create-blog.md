---
layout: post
title: 创建 blog 记录
categories: Blog
description: 将创建 blog 的过程记录下来
keywords: blog
---

## 选择博客蓝本  
选择 jekyll 或其他。
## 改变主题
将主题改为 `某某` 的并做自定义话设置
## 配置评论
未完成
## 可能遇到的错误
在部署博客的时候，某天打开发现页面的结构发生混乱，打开控制台才发现一些静态资源请求失败，比如：  
(加载失败图片)
``` 
https://frankwang1991.github.io/assets/vendor/jquery/dist/jquery.min.js
``` 
而还有一些同样的静态资源却加载成功了，仔细查看了下，加载失败的静态文件目录全部是在项目的 `assets/vendor` 下面，然后查看了项目的目录，发现并没有此目录文件夹。  
打开本地的代码库，发现此目录安安静静的躺着这里：  
![vendor 本地目录](https://raw.githubusercontent.com/FrankWang1991/images/master/SmoABz.png)  
那这问题很明显，找到了 `.gitignore` 文件：  
![.gitignore](https://raw.githubusercontent.com/FrankWang1991/images/master/YFYnPh.png)
将其中的 `vendor` 去掉即可。（CNAME 为自定义域名需要使用的，这里由于域名备案还未完成，先将 CNAME 文件放入此中来）。现在 `.gitignore` 应该是这个样子：
``` 
_site
.sass-cache
.jekyll-cache
.jekyll-metadata
CNAME
``` 

