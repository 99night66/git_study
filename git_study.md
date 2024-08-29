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

## 版本回退
- `git reset --hard HEAD^`：回到上一个版本
- `git reset --hard COMMITID前几位就行`：回退到指定版本 
- `git reflog`：记录你的每次命令

## 管理修改
- Git 跟踪并管理的是修改，而非文件
- 第一次修改 -> git add -> 第二次修改 -> git commit ：第一次的修改被提交，第二次的修改不会被提交
- `git diff HEAD --readme.txt`：可以查看工作区和版本库里面最新版的区别
- 第一次修改 -> git add -> 第二次修改 -> git add -> git commit ：这样才是正确的