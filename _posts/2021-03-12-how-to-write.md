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
1. 切换到你想要生成git本地仓库的目录 
```bash $ cd E:/Git/Git_documents```
2. 这里我选择直接clone一个项目到本地 
```bash 
$ git clone git://github.com/NeverMore2020/2021EBU6305G8.git
Cloning into '2021EBU6305G8'...
fatal: remote error:
  Repository not found.
```
什么情况。。后来发现是private 的原因，得另外加一些参数才可以。
```bash
$ git clone https://NeverMore2020:*********@github.com//NeverMore2020/2021EBU6305G8.git 
Cloning into '2021EBU6305G8'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 15 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (15/15), 3.51 KiB | 4.00 KiB/s, done.
```
**Done!**<br>
3. 切换到生成的本地项目目录下<br>
```bash
$ cd 2021EBU6305G8/
```

4. 试着创建一个新的branch<br>
这里说一下，branch这个分支功能可能是区分git和其他VCS的杀手锏
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
 \*的意思就是当前所在分支，另一个main分支是所clone的项目自带的origin分支<br>
7. 查看工作区中当前分支所具有的文件<br>
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ ls
README.md
```
竟然自带了main分支中的内容，原来当创建新的分支的时候，系统自动把当前分支中的内容copy到了新的分支。这里多说一句，当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。通俗一点说就是说你各个branch是动态的，本地文件夹（带.git文件的)就在那里允许你多个branch来回切换更新。
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
11. 本地修改一下cyttest分支的README.md<br>
再用git status 命令查看一下状态
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git status
On branch cyttest
Your branch is up to date with 'origin/cyttest'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
no changes added to commit (use "git add" and/or "git commit -a")
```
**Not staged** 说明我们的文件修改了但没有提交到暂存区，这里我们直接使用** git commit -am** 命令
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git commit -am 'README.md'
[cyttest 1fc114d] README.md
 1 file changed, 2 insertions(+), 1 deletion(-)
```
因为这个markdown文件我们之前提交到index暂存区过了(tracked)，所以直接使用这个命令即可提交新版本的markdown文件.<br>
不信我们试试** git commit -m**
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git commit -m 'README.md'
On branch cyttest
Your branch is ahead of 'origin/cyttest' by 1 commit.
  (use "git push" to publish your local commits)
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
no changes added to commit (use "git add" and/or "git commit -a")
```
可以看出，虽然该文件被track了，但没有在index暂存区中更新，因此使用该 **-m ** 命令无效。

12. 前面提到的track是什么意思呢？<br>
先创建一个新的test.php文件
```bash
$ touch test.php
```
现在这个文件是出于**untracked**状态的，如果直接commit将会报错
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git commit -m test.php
On branch cyttest
Your branch is ahead of 'origin/cyttest' by 2 commits.
  (use "git push" to publish your local commits)
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        test.php
nothing added to commit but untracked files present (use "git add" to track)
```
13.合并分支到main
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main)
$ git merge cyttest
Updating 583f837..c181b44
Fast-forward
 README.md | 5 +++--
 test.php  | 0
 test.txt  | 1 +
 3 files changed, 4 insertions(+), 2 deletions(-)
 create mode 100644 test.php
 create mode 100644 test.txt
```
合并成功，但是我们这里没有遇到**冲突**的情况：两个分支的同名文件都做了不同的修改之后再merge。<br>
14. merge冲突问题
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ vim README.md
//修改内容
$ git commit -am README.md
[cyttest 593ced4] README.md
 1 file changed, 1 insertion(+), 1 deletion(-)
//切换分支
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (cyttest)
$ git checkout main
Switched to branch 'main'
Your branch is ahead of 'origin/main' by 4 commits.
  (use "git push" to publish your local commits)
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main)
$ vim README.md
//修改内容
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main)
$ git commit -am 'README.md'
[main 3053177] README.md
 1 file changed, 1 insertion(+), 1 deletion(-)
```
现在我们再来merge
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main)
$ git merge cyttest
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```
冲突出现了，我们打开文件看看发生了什么
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main|MERGING)
$ cat README.md
\# EBU6305
\## Group Number: XXXXXXXX
\<<<<<<< HEAD
\## Project Title: 2XXXXXX
\=======
\## Project Title: 1XXXXXX
\>>>>>>> cyttest
```
貌似是git给我们自动加了点东西。。(上面那一串等号和>>>>>)。我们需要手动修改这些。
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main|MERGING)
$ vim README.md
```
修改完成
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main|MERGING)
$ git status -s
UU README.md
```
添加到暂存区
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main|MERGING)
$ git add README.md
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main|MERGING)
$ git status -s
M  README.md
```
再次提交
```bash
www@DESKTOP-ECGC35V MINGW64 /Git_documents/2021EBU6305G8 (main|MERGING)
$ git commit
[main 568029c] Merge branch 'cyttest' into main
```
Done!
