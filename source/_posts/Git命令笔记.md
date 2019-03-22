---
title: Git命令笔记
tags:
  - Git
categories:
  - Git
toc: true
abbrlink: 6204
date: 2019-03-22 23:17:45
---

## 创建Git版本库
```echo "# GitHexo" >> README.md
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/caochenhins/GitHexo.git
git push -u origin master

```
## git - 查看远程仓库信息
``` 
$ git remote show origin
* remote origin
  URL: git://github.com/schacon/ticgit.git
  Remote branch merged with 'git pull' while on branch master
    master
  Tracked remote branches
    master
    ticgit


```

## 删除git仓库
```
1.在本地仓库的目录下调用命令行删除根目录下的.git文件夹，输入
find . -name ".git" | xargs rm -Rf
这样本地仓库就清除了，像下面这样，master不见了。

2.手动删除掉残留的.git文件

3.在命令行中输入rm -rf + github仓库地址，例

rm -rf https://github.com/NeroSolomon/VLearning.git

4.在github的对应的库中到setting删除库。


```

## 删除项目里面所有.svn和.git 文件

```
项目代码上传至svn/git后会产生.svn/.git文件，项目打包时需要将这些文件删除。
打开终端，cd到项目文件夹
//删除文件夹下的所有 .svn 文件
 
find . -name ".svn"| xargs rm -Rf
 
//删除文件夹下的所有 .git 文件
 
find . -name ".git"| xargs rm -Rf

```

## git pull用法：
``` git pull命令的作用是：取回远程主机某个分支的更新，再与本地的指定分支合并。

一句话总结git pull和git fetch的区别：git pull = git fetch + git merge

git fetch不会进行合并执行后需要手动执行git merge合并分支，而git pull拉取远程分之后直接与本地分支进行合并。更准确地说，git pull使用给定的参数运行git fetch，并调用git merge将检索到的分支头合并到当前分支中。

基本用法：

git pull <远程主机名> <远程分支名>:<本地分支名>
1
例如执行下面语句：

git pull origin master:brantest
1
将远程主机origin的master分支拉取过来，与本地的brantest分支合并。

后面的冒号可以省略：

git pull origin master
1
表示将远程origin主机的master分支拉取过来和本地的当前分支进行合并。

上面的pull操作用fetch表示为：

git fetch origin master:brantest
git merge brantest
1
2
相比起来git fetch更安全一些，因为在merge前，我们可以查看更新情况，然后再决定是否合并。

```

## 把两个不同的项目合并
```
我在Github新建一个仓库，写了License，然后把本地一个写了很久仓库上传。


先pull，因为两个仓库不同，发现refusing to merge unrelated histories，无法pull


因为他们是两个不同的项目，要把两个不同的项目合并，git需要添加一句代码，在git pull，
这句代码是在git 2.9.2版本发生的，最新的版本需要添加--allow-unrelated-histories
git pull origin master --allow-unrelated-histories

```

## Git冲突文件问题
```
Pull is not possible because you have unmerged files.

本地的push和merge会形成MERGE-HEAD(FETCH-HEAD), HEAD（PUSH-HEAD）这样的引用。HEAD代表本地最近成功push后形成的引用。MERGE-HEAD表示成功pull后形成的引用。可以通过MERGE-HEAD或者HEAD来实现类型与svn revet的效果。

解决：

1.将本地的冲突文件冲掉，不仅需要reset到MERGE-HEAD或者HEAD,还需要--hard。没有后面的hard，不会冲掉本地工作区。只会冲掉stage区。

git reset --hard FETCH_HEAD

2.git pull就会成功。

对于博主的git reset --hard, 大家不要轻易用, 尤其小白, 这会把你的代码最新修改全部抹掉, 出现这种Pull is not possible because you have unmerged files的最好的解决办法：git add -u 你的修改目录
git add -u [path]表示 add to index only files modified or deleted and not those created.git add -u [path]: 把[path]中所有tracked文件中被修改过或已删除文件的信息添加到索引库。它不会处理untracted的文件。省略[path]表示.,即当前目录。
```


## git commit 报 "Changes not staged for commit

git commit -am "提交"

