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
git remote add origin https://github.com/caochenhins/HexoBack.git
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


## Git push -u orign master 提示hint: not have locally. This is usually caused by another repository push

一、情景
1.在GitHub上创建一个仓库A，并且初始化了readme.md这个文档.
2.在本地用Git Bash初始化仓库A(一开始没有从GitHub上拉下来).
git init /* 初始化一个空的仓库*/
3.在本地仓库新建一个文件 test.txt,并且提交到本地仓库.

git add test.txt /* 把test.txt设为仓库跟踪文件 */ 
git commit -m “测试第一次 test.txt” /* 提交文件并且追加备注 */

4.把本地仓库提交到远程仓库master 分支

git push git@github.com：用户名/仓库名 master 
提交失败：not have locally. This is usually caused by another repository push

二、原因
本地仓库跟远程仓库的版本不一样导致的,因为执行在步骤1的时候，远程的版本库会有个“commit readme.md”这个操作记录,本地仓库是不知道你有这个提交的，也就是说这个记录没在本地仓库是不存在的，所以俩个版本是不一致的.
三 、解决方法
A). 先更新本地版本在提交

利用 git pull 更新本地版本库.
再 利用git push命令把本地仓库推送至远程仓库.
B). 强制覆盖


加上 -f 参数 强制覆盖 
git push -f



## git如何移除某文件夹的版本控制
```
目录结构如下
project
    bin
    lib
    src
    ...... 
执行如下的操作
git add .
git commit -m "add bin/ lib/ src/"
git push origin master
 

突然发现原来 lib 目录不需要提交到版本库,但是现在远程已经存在该目录,what should I do.（吐出去的东西还能收回来吗）

万能的Git啊,help me！

功夫不负有心人,找到了解决问题的方法,其实就是 git rm 的命令行参数。

git rm 命令参数
-n --dry-run 
Don’t actually remove any file(s). Instead, just show if they exist in the index and would otherwise be removed by the command.
-r 
Allow recursive removal when a leading directory name is given. 
--cached 
Use this option to unstage and remove paths only from the index. Working tree files, whether modified or not, will be left alone.
解决方法
git rm -r -n --cached "bin/" //-n：加上这个参数，执行命令时，是不会删除任何文件，而是展示此命令要删除的文件列表预览。
git rm -r --cached  "bin/"      //最终执行命令. 
git commit -m" remove bin folder all file out of control"    //提交
git push origin master   //提交到远程服务器
此时 git status 看到 bin/目录状态变为 untracked

可以修改 .gitignore 文件 添加 bin/ 并提交 .gitignore 文件到远程服务器,这样就可以不对bin目录进行版本管理了。

以后需要的时候,只需要注释 .gitignore 里 #bin/ 内容,重新执行 git bin/ ,即可重新纳入版本管理。


```
