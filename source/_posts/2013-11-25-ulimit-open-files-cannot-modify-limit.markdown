---
layout: post
title: "Ulimit open files cannot modify limit"
date: 2013-11-25 18:49
comments: true
categories: [ulimit, linux, nofile] 
---

在使用Linux时我们经常会遇到文件描述符不够用的情况，特别是在进行网络编程时，这是因为Linux默认给一个用户分配的最大文件描述符数量`nofile`是1024
当系统中的文件描述符超过这个数时就会抛出错误。
<!-- more -->

要增加文件描述符我们可以使用`ulimit`命令，`ulimit`命令的主要用法有
* 查看系统对当前用户的所有限制 `ulimit -a`
* 查看文件描述符的软限制 `ulimit -Sn`,S表示soft
* 查看文件描述符的硬限制 `ulimit -Hn`,H表示hard
* 设置文件描述符限制 `ulimit -SHn 10000`, `ulimit -Sn 10000`, `ulimit -Hn 10000`

应限制一旦被设置了就不能再增加，软限制最多可以增加到和应限制一样的大小，一般应限制大于软限制。

## Root用户

如果当前用户是root，可以直接用`ulimit -SHn 10000`来讲文件描述符设置为10000
这中做法只对当前的会话有效，也就是说当关闭当前的terminal之后又会回到默认值，要永久的更改限制可以参考下面非root用户的做法。

## 非root用户

非root用户执行`ulimit -SHn n`时，如果n大于系统默认的限制，则会抛出错误：`-bash: ulimit: open files: cannot modify limit: 不允许的操作`，这是我
们可以通过修改`/etc/security/limits.conf`这个配置文件来改变系统的默认限制。在文件的末尾追加一下内容：

```sh
* soft nofile 4096
* hard nofile 4096
```

`*`代表除root以外的所有用户，也可以设置为特定用户的名称，`soft`表示软限制，`nofile`表示文件描述符，4096是新的默认值。

*PS:* ，有时候更改完后使用`ulimit -SHn`时仍然会报错，这是需要在`/etc/pam.d/common-session`中加入`session required pam_limits.so`
*再次PS* ，有时候经过上面的更改后使用`ulimit -n`会看到默认值并没有改变，我在ubuntu中遇到这种情况，解决办法是先使用`su username`登录当前用户，然后
就可以使用ulimit命令了。原因可能是gnome terminal默认是none-login的，所以我们在配置文件中的修改并没有影响到当前的terminal。
