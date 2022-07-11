---
title: "Git常用命令"
date: 2021-01-01T00:00:00+08:00
draft: false
tags: ['git', '基础']
categories: ['工具']
---
## 常用GIT命令整理

### 远程仓库相关命令

- 克隆仓库：$ **git** clone **git**://github.com/jquery/jquery.**git**
- 克隆仓库某分支：$ git clone -b 分支名 项目地址
- 查看远程仓库：$ **git** remote -v
- 添加远程仓库：$ **git** remote add [name] [url]
- 删除远程仓库：$ **git** remote rm [name]
- 修改远程仓库：$ **git** remote set-url --**push**[name][newUrl]
- 拉取远程仓库：$ **git** pull [remoteName] [localBranchName]
- **推送远程仓库：$ git push [remoteName] [localBranchName]**

### 分支(branch)操作相关命令

- 查看本地分支：$ **git** branch
- 查看远程分支：$ **git** branch -r
- 查看本地与远程分支：$ **git** branch -a
- **查看本地分支与远程分支的对应关系：$ git branch -vv**
- 创建本地分支：$ **git** branch [name] ----注意新分支创建后不会自动切换为当前分支
- 切换分支：$ **git** checkout [name]
- **创建新分支并立即切换到新分支：$ git checkout -b [name]**
- 删除分支：$ **git** branch -d [name] ---- -d选项只能删除已经参与了合并的分支，对于未有合并的分支是无法删除的。如果想强制删除一个分支，可以使用-D选项
- **合并分支：$ git merge [name] ----将名称为[name]的分支与当前分支合并(fast forward模式)，$ git merge --no-ff [name] ----不启用ff模式**
- 创建远程分支(本地分支**push**到远程)：$ **git push** origin [name]
- 删除远程分支：$ git push origin --delete [branchname]
- **从远程拉取分支到本地：git checkout -b xxx origin/xxx**

git checkout -b [新本地分支名]：创建以后执行git push会提示没有对应的远程分支关联：<br />
![](https://s2.loli.net/2022/07/11/Jmi7vgxdNPO53zo.png)<br />
这时候可以通过 **git push --set-upstream origin feature-v1.5 来进行关联**（假设本地分支为`feature-v1.5`），执行之后远程会自动新增对应的分支，然后进行关联。<br />
若创建以后直接执行：**git push origin [新本地分支名]** 就会在远程新创建一个分支并进行关联、推送。

### Tag相关指令

- 创建tag：git tag <tagName>
- 推送某个tag至远程：git push origin <tagName> 
- 推送全部tag至远程：git push origin --tags
- 根据commitId创建tag：git tag -a <tagName> <commitId>
- 查看某个tag信息：git show <tagName>
- 查看本地所有tag：git tag
- 查看远程所有tag：git ls-remote --tags origin
- 删除某tag：git tag -d <tagName>
- 删除远程tag：git push origin:<tagName>

### 其他常用

- git init：git 初始化
- git add readme.txt：添加
- git commit -m 'create readme.txt'：提交修改
- **git diff：当前与上次提交对比不同（工作区和暂存区）**
- git diff --cached：比较不同（暂存区和仓库）
- **git log：查看提交历史，有commit_id，用q退出**
- git reset --hard {commit_id}：穿梭过去与未来
- git push -f：强制提交至远程仓库
- git reflog：查看所用命令历史（以至于可从过去穿越到未来）
- git checkout -- 文件名：撤销修改
- git reset HEAD 文件名：把暂存区得修改撤销掉，重新放回工作区
- git rm 文件名：删除一个文件。误删了可以使用git checkout -- 文件名来恢复


### 关联远程仓库
在设置了SSH Key以后，

- git remote add origin git@server-name:path/repo-name.git：关联远程仓库
- git push -u origin master：第一次推送master分支得所有内容。之后就可以git push origin master了
- git clone 地址：克隆远程仓库
- git checkout -b dev：创建并切换到分支dev (git branch dev + git checkout dev)
- git branch：查看当前分支
- git merge dev：将dev分支合并到当前HEAD的指向上
- git branch -d dev：删除dev分支
- git switch -c dev：创建并切换到dev（更科学）
- git switch master：切换到已有的分支
- **git fetch：将远程主机的最新内容拉到本地，用户在检查了以后决定是否合并到工作本机分支中**
- **git pull：则是将远程主机的最新内容拉下来后直接合并，即：git pull = git fetch + git merge**

### 解决冲突
git merge dev发生冲突时，git会提示什么文件产生了冲突，进入相应文件修改冲突后，再git add，git commit就可以了。
