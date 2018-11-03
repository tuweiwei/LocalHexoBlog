---
title: git
date: 2018-10-25 22:50:21
categories: "工具"
tags:
    - git
    - 工具
description: git学习

---

查看、添加、提交、删除、找回，重置修改文件
```
git help <command> # 显示command的help
git show # 显示某次提交的内容 git show $id
git co -- <file> # 抛弃工作区修改
git co . # 抛弃工作区修改
git add <file> # 将工作文件修改提交到本地暂存区
git add . # 将所有修改过的工作文件提交暂存区
git rm <file> # 从版本库中删除文件
git rm <file> --cached # 从版本库中删除文件，但不删除文件
git reset <file> # 从暂存区恢复到工作文件
git reset -- . # 从暂存区恢复到工作文件
git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改
git ci <file> git ci . git ci -a # 将git add, git rm和git ci等操作都合并在一起做　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　git ci -am "some comments"
git ci --amend # 修改最后一git diff <branch1>..<branch2> # 在两个分支之间比较
git diff --staged # 比较暂存区和版本库差异
git diff --cached # 比较暂存区和版本库差异
git diff --stat # 仅仅比较统计信息
次提交记录
git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象
git revert HEAD # 恢复最后一次提交的状态
查看文件diff
git diff <file> # 比较当前文件和暂存区文件差异 git diff
git diff <id1><id2> # 比较两次提交之间的差异
查看提交记录
git log git log <file> # 查看该文件每次提交记录
git log -p <file> # 查看每次详细修改内容的diff
git log -p -2 # 查看最近两次详细修改内容的diff
git log --stat #查看提交统计信息
tig
Mac上可以使用tig代替diff和log，brew install tig
Git 本地分支管理
查看、切换、创建和删除分支
git br -r # 查看远程分支
git br <new_branch> # 创建新的分支
git br -v # 查看各个分支最后提交信息
git br --merged # 查看已经被合并到当前分支的分支
git br --no-merged # 查看尚未被合并到当前分支的分支
git co <branch> # 切换到某个分支
git co -b <new_branch> # 创建新的分支，并且切换过去
git co -b <new_branch> <branch> # 基于branch创建新的new_branch
git co $id # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除
git co $id -b <new_branch> # 把某次历史提交记录checkout出来，创建成一个分支
git br -d <branch> # 删除某个分支
git br -D <branch> # 强制删除某个分支 (未被合并的分支被删除的时候需要强制)
 分支合并和rebase
git merge <branch> # 将branch分支合并到当前分支
git merge origin/master --no-ff # 不要Fast-Foward合并，这样可以生成merge提交
git rebase master <branch> # 将master rebase到branch，相当于： git co <branch> && git rebase master && git co master && git merge <branch>
 Git补丁管理(方便在多台机器上开发同步时用)
git diff > ../sync.patch # 生成补丁
git apply ../sync.patch # 打补丁
git apply --check ../sync.patch #测试补丁能否成功
 Git暂存管理
git stash # 暂存
git stash list # 列所有stash
git stash apply # 恢复暂存的内容
git stash drop # 删除暂存区
Git远程分支管理
git pull # 抓取远程仓库所有分支更新并合并到本地
git pull --no-ff # 抓取远程仓库所有分支更新并合并到本地，不要快进合并
git fetch origin # 抓取远程仓库更新
git merge origin/master # 将远程主分支合并到本地当前分支
git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支
git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上
git push # push所有分支
git push origin master # 将本地主分支推到远程主分支
git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)
git push origin <local_branch> # 创建远程分支， origin是远程仓库名
git push origin <local_branch>:<remote_branch> # 创建远程分支
git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支
Git远程仓库管理
GitHub
git remote -v # 查看远程服务器地址和仓库名称
git remote show origin # 查看远程服务器仓库状态
git remote add origin git@ github:robbin/robbin_site.git # 添加远程仓库地址
git remote set-url origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址) git remote rm <repository> # 删除远程仓库
创建远程仓库
git clone --bare robbin_site robbin_site.git # 用带版本的项目创建纯版本仓库
scp -r my_project.git git@ git.csdn.net:~ # 将纯仓库上传到服务器上
mkdir robbin_site.git && cd robbin_site.git && git --bare init # 在服务器创建纯仓库
git remote add origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址
git push -u origin master # 客户端首次提交
```
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





