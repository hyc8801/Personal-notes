# git命令

## 查看分支

* git branch -l :查看本地分支
* git branch -r :查看远程分支
* git branch -a :查看全部分支（远程的和本地的）
* git branch -d <BranchName> 删除本地分支
* git branch -vv  查看本地分支关联的远端分支

## 查看当前的远程库

* git remote

## 切换分支命令

* git checkout -b v1 origin/v1  切换分支(v1 分支名)
* git checkout master 切换回之前分支

## 查看当前分支状态

* git status

## 先同步代码到本地

* git pull

## 提交至本地仓库

* git add .  提交所有
* git add a.txt 提交单个

## 提交已暂存的文件

* git commit -m '描述信息'

## 同步至服务器
* git push <远程主机名> <本地分支名>:<远程分支名> 
> 比如我要将本地的wy分支推送到远程wy分支 
git push origin wy:wy

## 与远程分支比较差异
* git rebase <远程分支名> 

## 查看文件改动内容
* git diff <文件名>

## 查看提交历史日志
*  git log （按Q键退出）

## 合并分支
* git merge <其他分支> 将 __其他分支__ 合并到当前分支
* git cherry-pick  <commit id>   
  > 将某次commit合并到当前分支  （commit id 可通过git log 查看）

## 版本回退
* git reset --hard HEAD^    回退到上一版本
* git reset --hard HEAD^^   回退到上上版本
* git reset --hard HEAD~100 回退到前100个版本
* git reset --hard <版本号>

## 查看版本号
* git reflog 

## 储藏当前工作区状态
* git stash         储藏当前工作区状态
* git stash list    查看储藏的状态列表
* git stash apply   恢复状态，但状态列表中扔存在。
* git stash drop    删除状态列表中的状态
* git stash pop     恢复状态并删除状态列表中状态

## 冲突总结

1. git rebase   重置节点（重置到最新节点）__禁止在公共分支上采用该方法__
2. 解决冲突
3. git rebase --continue 继续
4. 正常工作
