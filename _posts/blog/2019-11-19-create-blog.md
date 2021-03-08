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
**新增修改**：目前本人博客系统中使用 Gitment 初始化的时候会出现：

<div align="center">
    <img alt="gitment error" src="https://raw.githubusercontent.com/FrankWang117/images/master/jHxmLw.png">
</div>
还是推荐使用 Gitale。
**新增修改结束**。
因为上文提到的两个评论系统都是基于 GitHub 的 issus。支持在前端直接引入，不需要任何后端代码。可以在页面进行登录、查看、评论、点赞等操作，同时有完整的 Markdown / GFM 和代码高亮支持。尤为适合各种基于 GitHub Pages 的静态博客或项目页面。  
下面给出的链接是两者的官方 demo：  
    - Gitment demo ： [demo](https://imsun.github.io/gitment/)
    - Gitalk demo: [demo](https://gitalk.github.io/)

现在来看看如何给自己的 jekyll 博客添加相应的评论功能吧（以 Gitalk 为例）.

### 1. 创建 Github Oauth App

我们需要创建 Github OAuth App 来进行权限验证，可以点击 [此链接](https://github.com/settings/applications/new) 直达，也可以按照如下步骤：

在如图所示的 `Settings` 链接中：

<div align="center">
    <img alt="设置第一步" src="https://raw.githubusercontent.com/FrankWang117/images/master/github-setting.png">
</div>
点击下图所示区域的左下方的 `Developer settings`：  
<div align="center">
    <img alt="开发者设置" src="https://raw.githubusercontent.com/FrankWang117/images/master/D3fiyl.png">
</div>
点击左侧 `sidebar` 中的 `Oauth Apps` ,而后点击 `New OAuth App` 按钮之后，可以看到如下的页面：  
<div align="center">
    <img alt="Create Github app" src="https://raw.githubusercontent.com/FrankWang117/images/master/C4VkcT.png">
</div>
这就是我们需要为新的 GitHub App 配置的一些基础信息。  
其中的一些信息的填写情况是：

- **Application name** ： 输入应用名称，用户能理解并信任（Something users will recognize and trust）
- **Application description** ： 应用描述
- **Homepage URL**： 应用地址的全称，这里填写的是 `https://frankwang117.github.io`
- **Authorization callback URL** : 当用户权限验证之后的跳转路径，填写上文相同的地址即可
- ~~**Webhook URL** ： 当发生 `Events` 时向此地址发送 post 请求，具体可看[此文档](https://github.com/diandianxiyu/PageBlog/blob/master/%E4%BD%BF%E7%94%A8Git%E7%9A%84Webhooks%E8%BF%9B%E8%A1%8C%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%87%AA%E5%8A%A8%E9%83%A8%E7%BD%B2%E4%BB%A3%E7%A0%81.md)~~

~~最后点击 `Register application` 按钮即可创建一个新的 GitHub OAuth App 了。但是这是我们看到页面会出现~~

<div align="center">
<img alt="申请 private key" src="https://raw.githubusercontent.com/FrankWang117/images/master/VhlVHU.png" />
</div>  
~~所以我们需要按照提示，生成 `private key`。这时候我们的 `Client ID` 以及 `Client secret` 就可以使用了。~~

~~对了既然主要作为评论来使用这个 GitHub App，那就不要忘了修改一些权限（在此 App 设置页面的左侧 sidebar 第二项）~~
![Permissions & events](https://raw.githubusercontent.com/FrankWang117/images/master/wFRov1.png).

~~更多详细的内容可以[参考官方文档](https://developer.github.com/apps/building-github-apps/creating-a-github-app/)~~  
划掉的部分是生成 GitHub App 使用的，生成 GitHub OAuth App 则不需要更多的操作。

### 2. 设置博客配置信息

有了上面生成的 `Client ID` 以及 `Client secret` ，我们就需要向页面中添加相应的 `Gitalk` 代码了。  
首先是修改 `_config.yml`，添加以下代码：

```yml
gitalk:
  owner: frankwang117
  repo: frankwang117.github.io
  clientID: # 上述获得的 Client ID #
  clientSecret: # 上述获得的 Client secret #
```

然后是在 `_includes` 文件夹下创建 `comments.html`:

```html
<div id="gitalk-container"></div>
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css" />
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
<script>
  var gitalk = new Gitalk({
      id: '{{ page.url | truncate: 50, '' }}',
      clientID: '{{ site.gitalk.clientID }}',
      clientSecret: '{{ site.gitalk.clientSecret }}',
      repo: '{{ site.gitalk.repo }}',
      owner: '{{ site.gitalk.owner }}',
      admin: ['{{ site.gitalk.owner }}'],
      labels: ['gitment'],
      perPage: 50,
  })
  gitalk.render('gitalk-container')
</script>
```

基本这样的话，就可以在项目中看到了：

<div align="center">
    <img alt="评论区域" src="https://raw.githubusercontent.com/FrankWang117/images/master/VAfCeI.png">
</div>

不过这是初始化之后的。  
Gitalk 在使用时要为每篇博文都进行一次初始化，就是需要管理员（你创建 Gitalk 的 GitHub 账号）登录一下。  
PS：我的 GitHub 账号一直处于登录状态，发布文章后，我在预览的过程就实现了初始化，感觉影响不大。

如果你实在忍不了，可以看一下大佬实现的[自动初始化](https://draveness.me/git-comments-initialize)。

## 可能遇到的错误

在部署博客的时候，某天打开发现页面的结构发生混乱，打开控制台才发现一些静态资源请求失败，比如：

<div align="center">
    <img alt="静态文件缺失" src="https://raw.githubusercontent.com/FrankWang117/images/master/0RTV2u.png">
</div>
``` 
GET https://frankwang117.github.io/assets/vendor/primer-css/css/primer.css net::ERR_ABORTED 404
``` 
而还有一些同样的静态资源却加载成功了，仔细查看了下，加载失败的静态文件目录全部是在项目的 `assets/vendor` 下面，然后查看了项目的目录，发现并没有此目录文件夹。  
打开本地的代码库，发现此目录安安静静的躺着这里：  
<div align="center">
    <img alt="vendor 本地目录" src="https://raw.githubusercontent.com/FrankWang117/images/master/SmoABz.png">
</div>
那这问题很明显，找到了 `.gitignore` 文件：  
<div align="center">
    <img alt=".gitignore 文件内容" src="https://raw.githubusercontent.com/FrankWang117/images/master/YFYnPh.png">
</div>
将其中的 `vendor` 去掉即可。（CNAME 为自定义域名需要使用的，这里由于域名备案还未完成，先将 CNAME 文件放入此中来）。现在 `.gitignore` 应该是这个样子：

```
_site
.sass-cache
.jekyll-cache
.jekyll-metadata
CNAME
```
