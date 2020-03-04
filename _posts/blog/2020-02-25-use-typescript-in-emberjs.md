---
layout: post
title: 在 Ember.js app 中使用 TypeScript
categories: [TypeScript, ember.js]
description: 在 Ember.js app 中使用 TypeScript
keywords: ember, typescript,emberjs
---

TypeScript 在 JavaScript 的开发中占据着越来越重要的地位，也吸引着越来越多的开发者在项目中使用它。现结合使用 Ember.js 框架，如何配置使其能够使用 TypeScript。  


> ember cli version 3.16
>
> ember-cli-typescript version 3.1.3

## 生成新的项目

```powershell
ember new ember-quickstart --yarn && cd ember-quickstart
```

生成名为 `ember-quickstart` 的新 ember 项目，并进入

## 安装 typescript 插件

```shell
ember install ember-cli-typescript@latest
```

等待安装完成后，使用 `ember s` 启动项目，如果没有意外的话，可以看到命令行有一些报错，项目也是启动不起来：

![image-20200225163026828](https://tva1.sinaimg.cn/large/0082zybply1gc8qa7el73j318e0jdnpd.jpg)

造成这个现象的原因应该是 `typescript` 依赖导致的，这时退出命令，在命令行中执行：

```shell
yarn add typescript@3.7.5
```

重新启动项目。进入 [http://localhost:4200/](http://localhost:4200/) 即可以看到 ember 的欢迎界面。  

## 使用

生成路由：

```shell
ember g route scientist
```

![image-20200225165544415](https://tva1.sinaimg.cn/large/0082zybply1gc8r0l9cp6j30ey035416.jpg) 

可以看到生成的是  ts 类型的路由文件。

生成组件：

```shell
ember g component people-list
```

![image-20200225165724047](https://tva1.sinaimg.cn/large/0082zybply1gc8r26tjtij30f002gac0.jpg)

同样，也是 ts 类型的组件文件。  

## 在已经存在的 ember app 中使用 typescript

操作和上面的大致相同，安装 `ember-cli-typescript@latest` ,修改依赖中 `typescript` 的版本为 `3.7.5`。  

启动项目即可以正常运行。  

推荐的做法是先从一些无关紧要的地方开始添加类型说明。直到完全掌握类型，再深入到细微的地方，添加相关的类型。