---
layout: post
title: Git创建和使用branch
date: 2021-03-01
categories: blog
tags: [Git]
description: 文章金句。
---
# 如何使用命令行创建和使用branch
- git branch : 查看目前的branch
- git branch new_branch : 创建一个新的branch
- git checkout new_branch : 切换分支
- git checkout -b new_branch : 创建一个新的分支并且切换到新建分支
- git merge new_branch : 合并分支(注意要先切换到你想要合并成的branch下), 若报错需要加上**--allow-unrelated-histories**
- git branch -D new_branch : 删除分支

这里大家可以参考下面这篇博客的做法，写的很详细。
<a> https://www.cnblogs.com/yanliujun-tangxianjun/p/5740704.html </a>
