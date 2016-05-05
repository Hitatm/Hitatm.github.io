[TOC]
#主要命令
1. git config：配置相关信息
2. git clone：复制仓库
3. git init：初始化仓库
4. git add：添加更新内容到索引中
5. git diff：比较内容
7. git status：获取当前项目状况
8. git commit：提交
9. git branch：分支相关
10. git checkout：切换分支
11. git merge：合并分支
12. git reset：恢复版本
13. git log：查看日志

#git 的使用

## 配置git  
>git config --global user.name /user.email "name/email"
 将会在用户目录下面生成一个.gitconfig的配置文件.
 ** 如果不加--global 选项则会在当前目录下创建./git/config,从而只针对当项目的配置。**
## 获得||创建一个git仓库

* git clone [url] 或者 mkdir xxx ;cd xxx; git init

## 一般使用流程

工作流程
git的基本流程如下：
创建或修改文件
1. 使用git add命令添加新创建或修改的文件到本地的缓存区（Index）
2. 使用git commit命令提交到本地代码库
（可选，有的时候并没有可以同步的远端代码库）
3. 使用git push命令将本地代码库同步到远端代码库 。
4. git commit -a 参数可以检测到新建的但是没有 添加到缓存区的文件。
5. git rm 命令删除后会将被删除文件的信息添加到缓存区中，git commit 之后将会提交本地仓库删除文件。
6. git diff 文件那里做出修改可以查看当前文件和上次提交的文件不同之处 ，可以用--cached 查看提交到本地缓存区的文件不同。

## 分支与合并的
|标号|command|作用|
|-------|--------|------|
|1. |git branch xxx   |            创建一个新的分支 xxx|
|2.| git branch      |             产看当前的分支列表|
|3.| git checkout xxx |切换到分支xxx;|
|4. |git merge -m “备注信息”  xxx |合并当前分支与xxx分支|
|5. |git branch -d xxx|删除分支|

** git branch -d 只能删除那些已经被当前分支的合并的分支. 如果你要强制删除某个分支的话就用git branch –D**

> 如果是一个文件在两个分支中都做了修改，那么将会产生冲突，这时候git会总动将两者的不同标识出来。需要人工进行手动合并之后再提交。

## 分支回滚 （撤销之前的修改）
1. git reset --hard HEAD^ 向前回滚一次 

## 查看提交日志 
1. git log 显示所有的提交。git log filexxx 可以只查看filexxx的修改历史记录。
2. git log --since=<date> --after=<date> 可以按照日期查看
3. git log stat 查看文件深处了多少行 其实是类似于 git diff 的记录
4. git log --pretty=oneline 你也可用medium,full,fuller,email 或raw。 如果这些格式不完全符合你的相求， 你也可以用--pretty=format参数定义格式。
--graph 选项可以可视化你的提交图(commit graph)，会用ASCII字符来画出一个很漂亮的提交历史(commit history)线;

> $ git log --graph --pretty=oneline
5. git help log 有更多的查看选项。
