---
title: gitlab中ssh key的配置
date: 2017-07-24 21:06:29
tags: gitlab
---

>Vue的学习中，要求使用gitlab作为版本控制工具,在gitlab注册过程中，需要配置ssh key，翻阅资料后决定根据官方文档配置，避免出现不必要的错误。网络上还有一种使用putty生成密钥对的方法，我这边就不赘述了。

首先，需检查本机中是否存在ssh key对，我们打开Git Bash(本人习惯用Git Bash linux命令操作)，敲入下面命令：

```shell
cat ~/.ssh/id_rsa.pub
```