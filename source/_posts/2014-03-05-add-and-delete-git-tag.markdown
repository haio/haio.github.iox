---
layout: post
title: "Add and delete Git tag"
date: 2014-03-05 20:09
comments: true
categories: [git tag]
---

Git tag创建和删除指令集
<!-- more -->

## 创建

### 创建本地tag

```sh
git tag -a tagname description
或者
git tag tagname
```

### 将本地tag提交到服务器

```sh
git push --tag
```

## 删除

### 删除本地tag

```sh
git tag -d tagname
```

### 删除服务器上tag

```sh
git push --delete origin tagname
或者
git push  origin :refs/tags/tagname
```
