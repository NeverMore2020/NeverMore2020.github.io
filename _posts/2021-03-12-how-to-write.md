---
layout: post
title: Git创建和使用branch
date: 2021-03-12
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

这里大家可以参考下面<a href="https://www.cnblogs.com/yanliujun-tangxianjun/p/5740704.html">这篇博客</a>的做法，写的很详细。

# 可能遇到的烦人的问题
- fatal: unable to access 'https://github.com//NeverMore2020/2021EBU6305G8.git/': Failed to connect to github.com port 443: Timed out <br> 解决办法:ipconfig /flushdns(windows cmd 下运行), 可能原因是使用了VPN，改变了DNS路径，这个命令清除了DNS缓存，所以一切恢复正常了。

# Start from scratch: 从0创建git本地仓库
0. 安装git，配置github。(此处不再赘述)
1. 切换到你想要生成git本地仓库的目录 <br>
```bash $ cd E:/Git/Git_documents```
2. 这里我选择直接clone一个项目到本地 <br>
```bash 
$ git clone git://github.com/NeverMore2020/2021EBU6305G8.git
Cloning into '2021EBU6305G8'...
fatal: remote error:
  Repository not found.
```
什么情况。。后来发现是private 的原因，得另外加一些参数才可以。<br>
```bash
$ git clone https://NeverMore2020:*********@github.com//NeverMore2020/2021EBU6305G8.git 
Cloning into '2021EBU6305G8'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 15 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (15/15), 3.51 KiB | 4.00 KiB/s, done.
```
<br> **Done!**<br>
3. 切换到生成的本地项目目录下<br>
```bash
$ cd 2021EBU6305G8/
```
4. 试着创建一个新的branch<br>
这里说一下，branch这个分支功能可能是区分git和其他VCS的杀手锏<br>
```bash
$ git branch cyttest
```
5. 切换到新建的**cyttest**branch<br>
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main)
$ git checkout cyttest
Switched to branch 'cyttest'
```
6. 查看当前的branch<br>
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest) //可以发现括号里已经变成了当前的分支
$ git branch
* cyttest
  main
```
<br> \*的意思就是当前所在分支，另一个main分支是所clone的项目自带的origin分支
7. 查看工作区中当前分支所具有的文件<br>
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ ls

README.md
```
<br>竟然自带了main分支中的内容，原来当创建新的分支的时候，系统自动把当前分支中的内容copy到了新的分支。这里多说一句，当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。通俗一点说就是说你各个branch是动态的，本地文件夹（带.git文件的)就在那里允许你多个branch来回切换更新。
8. 给cyttest分支加一个txt文件<br>
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ echo 'test' > test.txt
//新建一个txt文件，并且向文件输出文本test
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git add .
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory
//'.'表示目录下的所有文件，add操作将指定的文件添加到index暂存区中
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git commit -m 'add test.txt'
[cyttest f4d96ab] add test.txt
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt
//当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，cyttest分支会做相应的更新。
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ ls
README.md  test.txt
//更新成功
```
9. 切换回main分支，发现并没有更新，这也就初步体现了branch的魅力：能够基于同一个本地文件分几条路径同时作出修改。<br>
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
```bash
$ git checkout main
Switched to branch 'main'
Your branch is up to date with 'origin/main'.

www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main)
$ ls
README.md
```
10. 试着push一下新建的cyttest分支<br>
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git push -u origin cyttest
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 12.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
remote:
remote: Create a pull request for 'cyttest' on GitHub by visiting:
remote:      https://github.com/NeverMore2020/2021EBU6305G8/pull/new/cyttest
remote:
To https://github.com//NeverMore2020/2021EBU6305G8.git
 * [new branch]      cyttest -> cyttest
Branch 'cyttest' set up to track remote branch 'cyttest' from 'origin'.
```

