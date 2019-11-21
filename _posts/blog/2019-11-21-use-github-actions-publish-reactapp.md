---
layout: post
title: 使用 Github actions 发布一个 React.js Application 到 Github pages 上
categories: 开发相关
description: 使用 GitHub actions 的相关功能将一个 React.js 应用发布到相应的 GitHub page 上去
keywords: Github,Github actions,React.js,react,Github pages,Github page
---

[Github actions](https://github.com/features/actions) 是 GitHub 推出的自动化构建平台，官方称之为 “Automate your workflow
from idea to production“。自己查阅资料，将使用 GitHub actions 发布一个 React.js Application 到 GitHub pages 的全过程实现并记录下来。  

## 前期准备  
我们现在默认对 GitHub actions 有一定的了解，如果不了解，可以看一下[官方文档](https://help.github.com/cn/actions) 了解相关细节。  
现在就开始使用 GitHub actions 发布 Reactjs application 到 GitHub pages 中。
### 1. 创建新的 repository
使用 GitHub 创建一个新的 repository 并进入：  
<div align="center">
    <img alt="新 repository 的页面" src="https://raw.githubusercontent.com/FrankWang1991/images/master/QRuUOz.png">
</div>
可以看到 repository 菜单栏的第四项是 `Actions` ,就代表着我们可以在这个项目中使用 actions 构建我们的应用，而如果想继续使用，则需要在此项目中设置 GitHub 密钥。  

### 2. 设置项目密钥  
我们将 GitHub 密钥放入项目菜单栏的最后项 `Settings` 中，并在新页面选择 `Secrets` 选项：
<div align="center"><img alt="添加 secret" src="https://raw.githubusercontent.com/FrankWang1991/images/master/pBvXVl.png"> </div>

可以看到，这里添加的 `secret` 名字是 `SECRET_TOKEN` ,我们需要记住这个名字，在之后的配置文件中通过这个变量名取到相应的值。  

## 项目生成与设置
我们将 GitHub 上的此项目 pull 到本地，然后使用命令行在此文件夹目录创建 React.js 项目：  

``` terminal
$ create-react-app freecodecamp
$ cd freecodecamp
```

打开项目的 `package.json` 文件：  

``` json
// ...
  "name": "freecodecamp",
  "version": "0.1.0",
  "private": true,
  "homepage": "https://frankwang1991.github.io/freecodecamp",
  "dependencies": {
    "react": "^16.12.0",
    "react-dom": "^16.12.0",
    "react-scripts": "3.2.0"
  }
// ...
```
主要是 `homepage` 属性的添加，为上文所示的格式，如果你设置过 GitHub pages，那你对这样的链接应该比较熟悉，如果没有，那格式可以总结为：`https://{yourGithubName}.github.io/{yourRepositoryName}`。  
然后新建项目文件  `.github/workflows/ci.yml`,其文件的内容是：  

``` yml
name: freecodecamp
on:
  push:
    branches:
      - master
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@master

    - name: Build and Deploy
      uses: JamesIves/github-pages-deploy-action@master
      env:
        ACCESS_TOKEN: ${{ secrets.SECRET_TOKEN }}
        BRANCH: gh-pages
        FOLDER: build
        BUILD_SCRIPT: npm install && npm run build
```

这个是使用的 [别人写好的模版](https://github.com/marketplace/actions/deploy-to-github-pages)，然后修改一些 `name` 以及 `ACCESS_TOKEN` 等属性。

## 提交并查看发布日志  
保存文件，将文件 push 到 GitHub 上去。  
打开项目所在 GitHub 页面：  
<div align="center"><img alt="查看 GitHub actions 日志" src="https://raw.githubusercontent.com/FrankWang1991/images/master/sAJloY.png"> </div>  
可以看到此项目正在编译以及相关的状态，单击项目名称，就可以查看具体的命令状态以及相应编译花费的时间：  
<div align="center">
<img alt="编译命令运行状态" src="https://raw.githubusercontent.com/FrankWang1991/images/master/nSK1hp.png"></div>

过一会查看我们设置的 `homepage` 的地址就可以看到我们的项目了：  
<div align="center">
<img alt="项目自动发布成功" src="https://raw.githubusercontent.com/FrankWang1991/images/master/51W5bT.png">
</div>  

## 总结  
如果知道了整个的配置文件，整个项目的配置过程还是很简单的。