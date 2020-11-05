# git 命令
<!-- TOC -->

- [git 命令](#git-命令)
  - [查看分支](#查看分支)
  - [查看当前的远程库](#查看当前的远程库)
  - [切换分支命令](#切换分支命令)
  - [查看当前分支状态](#查看当前分支状态)
  - [先同步代码到本地](#先同步代码到本地)
  - [提交至本地仓库](#提交至本地仓库)
  - [提交已暂存的文件](#提交已暂存的文件)
  - [同步至服务器](#同步至服务器)
  - [与远程分支比较差异](#与远程分支比较差异)
  - [查看文件改动内容](#查看文件改动内容)
  - [查看提交历史日志](#查看提交历史日志)
  - [合并分支](#合并分支)
  - [版本回退](#版本回退)
  - [查看版本号](#查看版本号)
  - [储藏当前工作区状态](#储藏当前工作区状态)
  - [标记（用于发布版本）](#标记用于发布版本)
  - [冲突总结](#冲突总结)
  - [提交错误分支（远端仓库回滚）](#提交错误分支远端仓库回滚)

<!-- /TOC -->
## 查看分支

- git branch -l :查看本地分支
- git branch -r :查看远程分支
- git branch -a :查看全部分支（远程的和本地的）
- git branch -d <BranchName> 删除本地分支
- git branch -vv 查看本地分支关联的远端分支

## 查看当前的远程库

- git remote 查看远端仓库

- git remote set-url origin [url] 修改远端仓库地址

## 切换分支命令

- git checkout -b v1 origin/v1 切换分支(v1 分支名)
- git checkout master 切换回之前分支

## 查看当前分支状态

- git status

## 先同步代码到本地

- git pull

## 提交至本地仓库

- git add . 提交所有
- git add a.txt 提交单个

## 提交已暂存的文件

- git commit -m '描述信息'

## 同步至服务器

- git push <远程主机名> <本地分支名>:<远程分支名>
  > 比如我要将本地的 wy 分支推送到远程 wy 分支
  > git push origin wy:wy

## 与远程分支比较差异

- git rebase <远程分支名>

## 查看文件改动内容

- git diff <文件名>

## 查看提交历史日志

- git log （按 Q 键退出）

## 合并分支

- git merge <其他分支> 将 **其他分支** 合并到当前分支
  > --squash 扁平化合并 （忽略之前的 commit 记录）
- git cherry-pick < commit id >
  > 将某次 commit 合并到当前分支 （commit id 可通过 git log 查看）

## 版本回退

- git reset --hard HEAD^ 回退到上一版本
- git reset --hard HEAD^^ 回退到上上版本
- git reset --hard HEAD~100 回退到前 100 个版本
- git reset --hard <版本号>

## 查看版本号

- git reflog

## 储藏当前工作区状态

- git stash 储藏当前工作区状态
- git stash list 查看储藏的状态列表
- git stash apply 恢复状态，但状态列表中扔存在。
- git stash drop 删除状态列表中的状态
- git stash pop 恢复状态并删除状态列表中状态

## 标记（用于发布版本）

- git tag <标签名称>    创建本地标签
- git push origin --tag  将本地标签推送远程仓库

## 冲突总结

1. git rebase 重置节点（重置到最新节点）**禁止在公共分支上采用该方法**
2. 解决冲突
3. git rebase --continue 继续
4. 正常工作

## 提交错误分支（远端仓库回滚）

1. git reset --hard <commit-id>
2. git push --force

> 本地撤销至某次提交，再强推至远端仓库，此方法慎用，会覆盖后面同事提交的代码
