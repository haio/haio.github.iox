---
layout: post
title: "Update the github fork"
date: 2013-12-03 18:28
comments: true
categories: [git, github, fork]
---

在Fork完一个项目后，有时我们会想要与原项目保持同步，我们可以通过以下步骤来实现：
<!-- more -->

## Track Remote

首先，将原始repo的url加入git remote中：

```sh
git remote add upstream git@github.com:user/repo.git
```

## Fetch Remote

然后获取原始repo的代码：

```sh
git fetch upstream
```

## Rebase

然后执行git rebase将原项目的改动更新到自己的fork，并且使当前项目中自有的commit位于当前branch的顶端：

```sh
git rebase upstream/master
```

如果你不想自己在fork中的commits被重写，可以把最后一条命令换为：

```sh
git merge upstream/master
```

## Push

最后将更新后的项目push到自己的fork中

```sh
git push -f origin master
```
