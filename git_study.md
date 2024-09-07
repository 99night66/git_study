## 初始化仓库
- 把当前的目录变成Git可以管理的仓库
`git init`

## 把文件添加到 Git 仓库
- commit 可以一次提交很多文件；所以可以多次 add 不同的文件
1. 把文件添加到仓库（添加到暂存区）：`git add remdme.txt`
2. 把文件提交到代码库（提交到当前分支）"`git commit -m "wrote a readme file"` 

## 查看仓库当前状态
`git status`

## 查看文件修改的具体内容
`git diff`

## 查看历史记录
- 分别显示从最近到最远的提交日志
`git log`
- `git log --pretty=oneline` : 第一个参数是commit id（版本号）
- 按 `q` 键 退出日志查看
- `git log --graph --pretty=oneline --abbrev-commit`：以图形化的方式显示 Git 提交记录（--abbrev-commit：显示缩短的提交哈希值，而不是完整的哈希值）

## 版本回退
- `git reset --hard HEAD^`：回到上一个版本
- `git reset --hard COMMITID前几位就行`：回退到指定版本 
- `git reflog`：记录你的每次命令

## 管理修改
- Git 跟踪并管理的是修改，而非文件
- 第一次修改 -> git add -> 第二次修改 -> git commit ：第一次的修改被提交，第二次的修改不会被提交
- `git diff HEAD --readme.txt`：可以查看工作区和版本库里面最新版的区别
- 第一次修改 -> git add -> 第二次修改 -> git add -> git commit ：这样才是正确的

## 撤销更改 
只能撤销修改的内容，如果已经 add 放到暂存区了，此时撤销就不会有变化
撤销回到最近一次 git commit 或者 git add时的状态(也可以说是丢弃工作区的修改)
- ``git checkout -- readme.txt` : 其中的`--`很重要，如果没有`--`，就会变成“创建一个新分支”的命令
如果已经 add 放到暂存区了
- `git reset HEAD readme.txt` : 可以把暂存区的修改退回到工作区
如果已经 commit 放到代码库中
- `git reset --hard HEAD^`：回到上一个版本

## 删除文件
- `rm file1.txt` : 删除文件也是一个修改操作
`rm file1.txt`之后，要从版本库中也删除 file1 文件 ：
- `git rm file1.txt`
- `git commit -m "删除 file1.txt 文件"`
- 
`rm file1.txt`之后，发现是误删，需要把文件恢复回来：
- `git checkout -- file1.txt`

如果`git rm file1.txt`删除了 file1 文件,需要恢复回来：
- `git restore file1.txt`
  
如果已经`git commit -m "删除 file1.txt 文件"`提交了删除操作，,需要恢复回来：
- `git checkout HEAD~1 -- file1.txt`: HEAD~1表示当前提交的前一个提交（只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的文件**）


## 创建SSH Key
`ssh-keygen -t rsa -C "youremail@example.com"`

## 添加远程仓库
`git remote add origin git@github.com:99night66/git_study.git`
- 添加之后，远程仓库的名字就是origin ，这是git默认的叫法
  
## 将本地库的内容推送到远程
`git push -u origin master`：把当前分支master 推送到远程origin

## 从远程克隆
- `git clone git@github.com:99night66/git_study.git`

## 创建与合并分支
创建分支：
- `git checkout -b dev`：`-b`参数表示创建并切换，相当于以下两条命令：
  - `git branch dev`：创建分支
  - `git checkout dev` ：切换分支
- `git branch`：查看当前分支
- `git merge dev`: 将 dev 分支合并到当前分支（合并分支之前要确保两个分支都已经提交更改）
- `git branch -d dev`：删除 dev 分支

## 解决冲突
当两个已经提交更改的分支合并时，发现了`conflict`冲突时，进行如下几步：
1. `git status` : 查找冲突的文件是哪些
2. 查看冲突的文件
3. 手动修改冲突文件的内容
4. 重新 `add` & `commit` 提交

## 分支管理策略
通常，合并分支后，如果可能，git 会用 `fast forward` 模式，但这种模式下，删除分之后，会丢掉分支信息

123