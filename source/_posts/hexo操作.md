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

# hexo上使用图片
1. 把主页配置文件`_config.yml`里的`post_asset_folder:`这个选项设置为`true`
2. 在你的hexo目录下执行这样一句话`npm install hexo-asset-image --save`，这是下载安装一个可以上传本地图片的插件，来自[dalao](https://github.com/CodeFalling/hexo-asset-image)
3. 等待一小段时间后，再运行`hexo n "xxxx"`来生成md博文时，`/source/_posts`文件夹内除了`xxxx.md`文件还有一个同名的文件夹
4. 最后在`xxxx.md`中想引入图片时，先把图片复制到xxxx这个文件夹中，然后只需要在xxxx.md中按照markdown的格式引入图片：`![你想输入的替代文字](xxxx/图片名.jpg)`
注意： xxxx是这个md文件的名字，也是同名文件夹的名字。只需要有文件夹名字即可，不需要有什么绝对路径。你想引入的图片就只需要放入xxxx这个文件夹内就好了，很像引用相对路径。
5. 最后检查一下，`hexo g`生成页面后，进入`public\2017\02\26\index.html`文件中查看相关字段，可以发现，html标签内的语句是`<img src="2017/02/26/xxxx/图片名.jpg">`，而不是`<img src="xxxx/图片名.jpg>`。这很重要，关乎你的网页是否可以真正加载你想插入的图片。

>ps.添加以上功能，我应该是参考一位小姐姐的博客，为什么这样说呢，因为她的博客充满了表情包，试想，一个男生应该不会这么可爱的吧。[小姐姐博客](https://blog.csdn.net/sugar_rainbow/article/details/57415705)

附：[在另一台电脑上搭建Hexo环境](https://www.jianshu.com/p/f4cc5866946b)