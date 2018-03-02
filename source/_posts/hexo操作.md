---
title: hexo操作
date: 2018-02-26 11:11:59
tags: hexo
---

>俗话说，工欲善其事，必先利其器，2018年给自己定的主要目标就是学习，那么首先就必须有做笔记的好习惯，所以要求自己先熟练的掌握做这个做笔记的工具，才能开始接下来的学习。
<!--more-->

废话不多说，开始整理hexo博客的几个技术点：

# 掌握Markdown语法
Markdown语法主要参考网上文档：
* [Markdown语法1](https://www.jianshu.com/p/82e730892d42)
* [Markdown语法2](https://www.jianshu.com/p/b03a8d7b1719)
* [Markdown语法3(完全版)](http://wowubuntu.com/markdown/index.html)
* [Markdown语法4(快速版)](http://wowubuntu.com/markdown/basic.html)

# 常用命令
1. 本地写博客
```shell
hexo clean
hexo g
hexo new post "xxx"
hexo s
```

1. 提交到github上
```shell
git status
git add -A
git status
git commit -m "mark"
git push repoName master
```

1. 提交静态页面
```shell
hexo clean
hexo d -g
```

1. 在另一台电脑上使用hexo
```shell
git init
git remote add hexo repoName
git pull hexo master
```

附：[在另一台电脑上搭建Hexo环境](https://www.jianshu.com/p/f4cc5866946b)