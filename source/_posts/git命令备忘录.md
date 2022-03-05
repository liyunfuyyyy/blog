---
title: git命令备忘录
date: 2022-03-05 11:08:36
categories: 知识点
tags: ['git','备忘录']
---

- git init 初始化版本库
- git add 每次提交前都要添加
- git commit -m "修改信息"  
- git log 打印提交记录
- git diff readme  查看版本区别
- git reset 回到某个版本  `git reset --hard  版本代号`
- git reflog 打印操作记录 再使用`git reset 版本号`可以到未来
- 工作区的文件`git add`之后到了暂存区，暂存区`git commit`一次性提交到`master`分支
- `git diff HEAD -- readme.txt` 查看工作区和版本库里面最新版本的区别
- `git checkout -- readme.txt` 丢弃工作区的修改，从暂存区恢复
- `git reset HEAD readme.txt`暂存区回到上一个版本
- `git remote add origin git@github.com:michaelliao/learngit.git` 关联远程仓库
- `git push -u origin master`第一次推送
- `git push origin master`以后推送
- `git remote -v`查看远程仓库
- `git remote rm origin`解除远程仓库
- `git switch -c dev`创建并切换到新的`dev`分支
- `git switch master`切换到master分支
- `git branch`查看分支
- `git branch -d dev`删除分支
- `git merge dev`将dev分支合并到当前分支上
- `git merge --no-ff -m "merge with no-ff" dev`合并分支并禁用快速合并
- `git stash`把当前工作现场储存下来，方便下一次恢复现场继续工作 
- `git stash pop`恢复现场，并把stash内容删除
- `git cherry-pick <commit>`  如果当前也有bug 就把原先的提交复制到这儿一份
- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。
