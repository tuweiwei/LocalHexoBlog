---
title: git
date: 2018-10-25 22:50:21
categories: "工具"
tags:
    - git
    - 工具
description: git学习

---


用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别

如果远程仓库为空，本地仓库和其关联后，使用命令git push -u origin master第一次推送master分支的所有内容；

若远程仓库有文件，可能会出现如下异常
git 异常：

解决  fatal: refusing to merge unrelated histories

Github新建一个仓库，如果本地有一个仓库。此时想把本地仓库上传到git

首先想到的是pull，因为两个仓库不同，让本地先把远程的文件拷下来，在一起提交，却发现refusing to merge unrelated histories，无法pull


因为他们是两个不同的项目，即使此时指定了远程仓库，把本地和远程已经联系了起来  要把两个不同的项目合并，git需要添加一句代码，需要添加--allow-unrelated-histories
git pull origin master --allow-unrelated-histories


# 版本回退
git log 查看历史每一次提交过的版本，由近到远， 标有HEAD的为当前版本
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交，上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

git reset --hard HEAD^

如果此时又想回到回退之前当时的版本 想恢复 命令行窗口没关，可以找到当时的commitid
git reset --hard commitid
但是 命令行窗口关了 不知道当时的commitid了 怎么办
Git提供了一个命令git reflog用来记录你的每一次命令

不过git reset 这是回到过去，查看，一般回退版本是要提交的，所以还是revert + push 才是正解:

使用 git revert <commit_id>操作实现以退为进，
git revert 不同于 git reset  它不会擦除"回退"之后的 commit_id ,而是正常的当做一次"commit"，产生一次新的操作记录，所以可以push，不会让你再pull


# 撤销修改  --未提交
git checkout -- file可以丢弃工作区的修改：
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次git commit或git add时的状态。
git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令



用命令git reset HEAD <file>可以把暂存区的修改撤销掉（unstage），重新放回工作区,然后再用git checkout -- file 丢掉工作区的修改


场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。



# 分支
对分支的操作实质上对指针的操作
新建dev分支： git checkout -b dev
切换   分支： git checkout master
合并    分支：用于合并指定分支到当前分支 如在dev分支工作了很久， 优先于master分支，切换回master分支后 git merge dev 将dev分支的内容合并到master分支，实际上将master指针指向dev分支的最新提交
删除本地分支   ：  git branch -d dev 删除dev指针
删除远程分支 ： git push origin --delete Chapater6   

住： git checkout -b dev 相当于 创建分支 git branch dev 和 切换分支 git checkout dev

git branch 会列出所有分支 当前分支会标有 *
git remote -v 查看远程库的信息
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并
git merge --no-ff -m "merge with no-ff" dev


开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。


# 冲突
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并即完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
用git log --graph命令可以看到分支合并图。





