---
title: linux 常用文件和文本操作
tags:
  - linux
  - 文件
categories:
  - linux
keywords:
  - linux文件操作
  - linux初学者学习
type: 原创
toc: true
date: 2021-11-12 11:26:24
updated: 2021-11-12 11:26:24
---

## 文件操作

### touch 创建文件

创建指定名称后缀的文件

> touch test
> touch test.conf

### cp 复制

1.必须的目标文件名
> cp test1.conf  test2.conf

2.递归复制
包含子目录时用，不加 -r 的话，目录包含子目录文件的话，就会报错
> cp -r origin dest

3.复制目录内文件
复制目录内的文件到别一个目录内，但不包括test目录内的隐藏文件，子目录内的隐藏文件会被复制
> cp -r `/root/test/*`  `/usr/test`

### mv 移动、重名命、备份

1.移动文件到 /tmp
> mv test.conf  /tmp

2.备份文件
> mv test test.backup

3.排除指定目录
> mv !(child1|child2) child1

如查报错
> -bash: !: event not
执行下面命令解决
> shopt -s extglob

### rm 删除文件

1.有提示删除
> rm  test1.conf

2.不提示，直接删
> rm -f test.conf

3.删文件夹时加-r,不要提示则 -rf
> rm -r dir

4.删除目录不提示 -f 不提示
> rm -rf test/

## 文本操作

### cat 查看文件

这个很常用，直接将结文件结构输出

1.查看文本
> cat test.log

2.合并新文件
如果有两个log文件，想要合并成一个，可以用这个用法
> cat log1.txt log2.txt > newFile.txt

3.追加写入
这种方式一般用在脚本居多，用来写入其他shell 命令。
如果使用的是 `>` 就是覆盖，如果是`>>` 就是追加。

> cat > test.log << "EOF"
1111
2222
3333
> EOF

4.统计一个词的出现频次
这个就很常用了，平是上服务器上查日志，想知道某个关键字出现几次，就可以用这个来统计。
> cat text.log | grep 'aabb' |wc -l

