---
title: vue学习以及metis的使用
date: 2017-08-01 16:31:17
tags: Vue
---

>vue框架是用于前端开发的一个非常好用框架，配合公司自己搭建的metis能够很方便的编写前端代码。

#Vue学习
>在学习前需要事先安装git和node.js，具体方法这里不赘述，可自行百度，非常简单。学习过程在VSCode上编写的，安装最新版本即可。

##使用命令行创建项目
打开VSCode中的命令行或者node.js或者git bash，依次输入如下命令：

```shell
# 全局安装 vue-cli
$ npm install --global vue-cli
# 创建一个基于 webpack 模板的新项目
$ vue init webpack my-project
# 安装依赖，走你
$ cd my-project
$ npm install
$ npm run dev
```