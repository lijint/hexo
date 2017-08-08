---
title: vue学习以及metis的使用
date: 2017-08-01 16:31:17
tags: Vue
---

>vue框架是用于前端开发的一个非常好用框架，配合公司自己搭建的metis能够很方便的编写前端代码。
<!--more-->

# Vue学习
>在学习前需要事先安装git和node.js，具体方法这里不赘述，可自行百度，非常简单。学习过程在VSCode上编写的，安装最新版本即可。
<!--more-->
## 使用命令行创建项目
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

这样一个程序员最喜欢的helloworld项目就大功告成了，vue的知识点可以在[vue.js官网](https://cn.vuejs.org/)或者[菜鸟教程](http://www.runoob.com/vue2/vue-tutorial.html)里学习。

# Metis的使用
>Metis是公司基于智能pos专门开发的vue框架，通过调用组件实现高效率的前端界面实现。

## 安装
这里先介绍在vue框架中加入metis，首先，安装最新稳定版的组件，使用npm安装

```shell
npm install "git+http://git.landicorp.com/metis/metis-npm.git" --save-dev
```

随后，引入配置，工程中打开webpack.conf.js文件，按按需加载配置的方式加入以下代码：

```javascript
var merge = require('webpack-merge')
var metisWebpackConfig = require('metis/webpack.metis.conf.js')  // 引入 Metis 配置

// 合并项目配置跟 Metis 配置
merge(metisWebpackConfig(utils), {  //  这里可传入 utils，utils 主要实现方法 assetsPath，也可以不传入
    //  项目其他webpack配置
})
```