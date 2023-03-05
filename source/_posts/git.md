---
title: Git
date: 2023-03-05 10:00:21
categories:
- Git
tags:
- Git
---
### Git本地操作
1. 用户信息配置(--global：当前机器所有仓库都会使用该配置，也可以对某一个仓库单独指定)
> git config --global user.name "Your Name"  
> git config --global1 user.email "email@example.com"
2. 仓库初始化
> git init
3. 提交
> git add file  
> git commit -m "message"
4. 提交文件状态查看
> git status
5. 提交文件修改的内容
> git diff file
6. git提交日志
> git log  
> git log --pretty=oneline 一行简洁显示
7. 版本回退(HEAD代表当前版本，HEAD^代表上一个版本，更多的就是HEAD~100；commit id：版本号)
> git reset --hard HEAD^  回退到上一版本  
> git reset --hard commit id 回退到该版本号的状态
8. 提交命令记录
> git reflog
9. 工作区和版本库中最新版本的区别
> git diff HEAD -- readme.txt
10. 丢弃工作区的修改
> git checkout -- file name
11. 暂存区的修改内容撤销，重新放回工作区
> git reset HEAD file name
12. git删除文件（也可以手动删除，再git add）
> git rm file name
13. 误删除文件还原（版本库中还有的话）
> git checkout -- test.txt (即丢弃工作区中的修改)

### Git提交
1. windows添加SSH-KEY
> ssh-keygen -t rsa -C "youremail@example.com"
2. 提交流程
> git add (提交到暂存区：stage) => git commit(将所有暂存区的内容提交到本地的当前分支)
3. 添加远程库(origin 后面修改成你自己的仓库地址，修改后origin地址就指向你的远程仓库)
> git remote add origin git@github.com:michaelliao/learngit.git
4. 克隆远程仓库
> git clone git@github.com:michaelliao/gitskills.git

### Git分支管理
1. 创建分支 (并切换到该分支)
> git branch dev（创建分支）  
> git checkout -b dev  创建分支 (并切换到该分支)
2. 切换分支
> git checkout dev（切换分支）
3. 查看分支
> git branch
4. 删除分支
> git branch -d dev
5. 合并分支
> git checkout dev  
> git merge master  
> git merge --no-ff -m "merge with no-ff" dev （--no-ff在删除分支后不会丢失分支信息，fast forward 模式看不出曾经做过合并）
6. 切换分支（新）
> git switch -c dev (创建并切换到新分支)
> git switch master (切换分支)
#### Feature分支
1. 新功能开发，最好拉一个feature分支，开发后删除
2. 需要强行删除，git branch -D feature-branch

### Git Bug分支管理
1. 隐藏工作区
> git stash
2. 隐藏的内容查看
> git stash list
3. stash内容恢复
> git stash apply (恢复后，stash内容不能删除，需要git stash drop删除)  
> git stash pop (恢复的同时，把stash内容也删除了)
4. 复制一个特定的提交到当前分支：cherry-pick
> 在master分支上修复的bug，想要合并到当前dev分支，可以用git cherry-pick <commit>命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

### Git多人协作
1. 查看远程库信息
> git remote (或者git remote -v 显示更加详细的信息)
2. 推送分支
> git push origin master
3. 抓取远端分支
> git pull origin branch
3. 创建分支并关联远端
> git checkout -b dev origin/dev  (在本地创建和远程分支对应的分支)  
> git branch --set-upstream branch-name origin/branch-name (关联远端分支)
4. rebase
> git rebase (变基：开发很少用)  
> 把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了  
> rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。