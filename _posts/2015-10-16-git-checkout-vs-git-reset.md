---
layout: post
category: "Git Learning"
title: "git checkout vs git reset"
tags: [Git]
---

首先，这两个命令都很常用，比如`git checkout`用来切换branch，`git reset HEAD filename`用来unstage一个文件。
以下对两者在撤消更改和回退历史版本的应用中的不同略做总结。

## 撤消更改，针对files
<!--more-->
- `git reset -- files`：将stage中的更改撤消，即将之同步为History(last commit)的样子
- `git checkout -- files`：将working dir中的文件更改撤消，即将之同步为stage中的样子(正常情况下，stage和last commit指向的内容相同)

但是reset的不同参数可以有不同的效果，`--hard`会同时附加撤消working dir的更改，所以我们用`git reset --hard HEAD`将working dir全部恢复成HEAD(last commit)指向的内容。但需要注意`--hard`参数不适用于个体文件(path)

## 回退版本，针对files

- `git reset <commit> files`：只将stage中files回退至`<commit>`版本的files内容
- `git checkout <commit> files`：将`<commit>`版本的files中签出，working dir和stage同时更改，相对HEAD有了新变化

实际应用中第一种情况，此时`git st`会显示同一文件既出现在stage中，又需要加入stage中，就好像是我们更改了某一文件，加入了stage，随后又做了第二次更改，但还未加入stage，但区别是此处working dir与history相同，只有stage回退了，所以`git diff`(working dir相对于stage的不同)显示的内容与`git diff --cached`(stage相对于history的不同)正好相反。

两者都未移动HEAD。

## 回退版本，针对`<commit>`

首先我们假设某次commit之后，处于master(HEAD->master)分支，working dir干净。然后我们试图回退到某一历史版本(比如master^3)
- `git reset [--hard] master^3`：HEAD->master分支同时移动，stage与history同步都变，若`--hard`捎带workding dir也变
- `git checkout master^3`：master不动，HEAD回退(无branch的话多半是detached HEAD模式了)，stage与working dir同步都变，相对HEAD无变化

两者都会移动HEAD。

