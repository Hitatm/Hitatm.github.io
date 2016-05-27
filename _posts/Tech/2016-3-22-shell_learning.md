---
layout: post
title: Shell 基础
category: Tech
tags: Git
keywords: 
description: git tools.
---

# shell基础
1. shell 环境变量

|命令 | 说明|
|--------|----------|
|set| 显示当前 Shell 所有环境变量，包括其内建环境变量（与 Shell 外观等相关），用户自定义变量及导出的环境变量|
|env |显示与当前用户相关的环境变量，还可以让命令在指定环境中运行|
|export| 显示从 Shell 中导出成环境变量的变量，也能通过它将自定义变量导出为环境变量|



2. shell 变量 declare 声明一个变量

* $符号用于表示引用一个变量的值


3. 变量修改&删除

>变量的修改有以下几种方式：

|变量设置方式|  说明|
|---------|-----------|
|$\{变量名#匹配字串\} |从头向后开始匹配，删除符合匹配字串的最短数据|
|$\{变量名##匹配字串\} |   从头向后开始匹配，删除符合匹配字串的最长数据|
|$\{变量名%匹配字串\} |从尾向前开始匹配，删除符合匹配字串的最短数据|
|$\{变量名%%匹配字串\}  |  从尾向前开始匹配，删除符合匹配字串的最长数据|
|$\{变量名/旧的字串/新的字串\}  |  将符合旧字串的第一个字串替换为新的字串|
|$\{变量名//旧的字串/新的字串\}  | 将符合旧字串的全部字串替换为新的字串|

>变量删除

可以使用unset命令删除一个环境变量
4. 环境变量生效
source command 