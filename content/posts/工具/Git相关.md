---
title: "Git相关"
date: 2021-01-01T00:00:00+08:00
draft: false
tags: ['git', '基础']
categories: ['工具']
---
## 概述
在实际开发中，我们应该按照几个基本原则进行分支管理：<br />**首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；**<br />一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：<br />![](https://cdn.nlark.com/yuque/0/2020/png/714353/1593323630249-43d22b78-5340-473e-8ad3-22bd6f5368b8.png#align=left&display=inline&height=151&margin=%5Bobject%20Object%5D&originHeight=151&originWidth=301&size=0&status=done&style=none&width=301)<br />当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：<br />![](https://cdn.nlark.com/yuque/0/2020/png/714353/1593323643819-2c9ab2e1-fd60-4b3d-8290-2fc663b16200.png#align=left&display=inline&height=233&margin=%5Bobject%20Object%5D&originHeight=233&originWidth=367&size=0&status=done&style=none&width=367)

## 操作

### 创建与合并分支

- 创建分支：
```git
$ git branch dev // 创建dev分支
```

- 切换分支：
```git
$ git checkout dev  // 切换到dev分支
```
切换分支现在用switch命令更多：
```git
$ git switch dev // 直接切换到已有的dev分支
$ git switch -c dev // 创建并切换到新的dev分支
```

- `git checkout`命令加上`-b`参数表示创建并切换到dev：
```git
$ git checkout -b dev
// 相当于
$ git branch dev // 创建dev分支
$ git checkout dev  // 切换到dev分支
$ git branch // 查看当前分支
```

- `git merge`命令用于合并指定分支到当前分支（head指向的分支）：
```git
$ git merge dev //表示将dev分支合并到当前分支，当前分支就是head所指向的分支
```

- 删除`dev`分支：
```git
$ git branch -d dev // 删除dev分支
```

### 解决冲突
当分支上有不同内容提交且内容有冲突时，Git无法执行“快速合并”，必须手动解决冲突后再提交。<br />可以使用`git status`查看冲突的文件，然后直接进入文件，Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，修改之后再提交即可。 

### 分支管理策略
合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。<br />如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。<br />准备合并`dev`分支，请注意 `--no-ff`参数，表示禁用`Fast forward`：
```git
$ git merge --no-ff -m "merge with no-ff" dev
```
![image.png](https://cdn.nlark.com/yuque/0/2021/png/714353/1617953297386-fb78e1f1-d376-4e37-a45d-5bb0baf2cc9f.png#align=left&display=inline&height=313&margin=%5Bobject%20Object%5D&name=image.png&originHeight=607&originWidth=732&size=70884&status=done&style=none&width=377)

### Bug分支
每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。<br />当接到需要修复bug的任务时，当前工作还未做完且没法提交，这时候可以使用 `stash` 功能，把当前工作现场“储藏”起来，等以后可以恢复现场继续工作：
```git
$ git stash
```
然后再 `git status` 查看状态就是干净的。接下来可以开始放心修复bug了。<br />首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：
```git
$ git checkout master
$ git checkout -b issue-101
```
修复bug并完成提交后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：
```git
$ git switch master
$ git merge --no-ff -m "merged bug fix 101" issue-101
```
修复完成后，可以切换或 `div` 继续干活了：
```git
$ git switch dev
$ git stash list // 查看之前保存的工作现场
```
需要恢复工作现场，有两种办法：

- 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；
- 另一种是用`git stash pop`，恢复的同时把stash内容也删了。

我们在master中修复了bug，但dev是从早期master中分支出来的，也存在着bug待修复。为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：
```git
$ git branch
$ git cherry-pick <commit>  // 把bug提交的修改“复制”到当前分支，避免重复劳动
```

### Feature分支
每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。<br />若所创建的分支还未合并且需要删除，使用 `git branch -d feature-vulcan` 未被合并，改用 `-D` 来进行删除。

### 多人协作
当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。<br />查看远程库的信息，用 `git remote` 或 `git remote -v` ；

#### 推送分支
> 
#### [多人协作具体场景](https://www.liaoxuefeng.com/wiki/896043488029600/900375748016320)

并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；<br />
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；<br />
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；<br />
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；<br />
1. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；<br />
1. 如果合并有冲突，则解决冲突，并在本地提交；<br />
1. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

### 场景

#### 与远程关联
当远程有一个分支时，本地创建一个分支与远程进行关联：
```git
$ git checkout --track origin/feature/v1.0.4 // 创建本地分支并与同名远程分支关联
```

#### 版本回退
使用 `git log` 或 `git reflog` 查看需要回退的版本，然后使用 `git reset --hard [commit id]` ，最后 `git push -f` 进行强行推送。

### Rebase
rebase操作的特点：把分叉的提交历史“整理”成一条直线，看上去更直观。缺点是本地的分叉提交已经被修改过了。

![](https://cdn.nlark.com/yuque/0/2020/jpeg/714353/1594109344856-05e850b2-9f45-4749-9251-4259858ab0c0.jpeg#align=left&display=inline&height=227&margin=%5Bobject%20Object%5D&originHeight=227&originWidth=800&size=0&status=done&style=none&width=800)
