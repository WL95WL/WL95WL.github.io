---
title: Mysql索引底层数据结构与算法
date: 2019-10-08 15:46:42
tags: mysql
comment: true
---

# 		Mysql索引底层数据结构与算法

## 1

数据结构：B+trees

非叶子节点不存储data 只存储key

叶子节点不存储指针

Mysql的存储引擎

MyISAM (非聚集索引)

.frm  存储表结构

.MYD  表数据

.MYI    索引 b+trees

## InnoDB(聚集索引 ==主键索引)

.frm表结构

.ibd 索引+数据

mysql 节点默认16k



为什么InnDB表必须有主键，并且推荐使用整性的自增主键？

使用UUID当做主键存在的问题：1.占用空间、效率低  UUID还需要转换为ascii码   2.UUID会造成数的撕裂、整型递增直接在后面追加   UUID不一定递增，会造成data分裂后在插入

