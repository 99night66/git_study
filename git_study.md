# 本地学习 git
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


# 远程学习git
## 创建SSH Key
`ssh-keygen -t rsa -C "1546383065@qq.com"`

## 添加远程仓库：
`git remote add origin git@github.com:99night66/git_study.git`：添加之后，远程仓库的名字就是origin ，这是git默认的叫法

## 将本地库的内容推送到远程
`git push -u origin master`：把当前分支master 推送到远程origin

## 从远程克隆
- `git clone git@github.com:99night66/git_study.git`

## 从远程拉取分支到本地（本地是空文件夹）
1. 从远程克隆：`git clone git@github.com:99night66/git_study.git`
2. 进入clone的目录：`cd git_study`
3. 查看远程分支：`git branch -r`

## 创建与合并分支
创建分支：
- `git checkout -b dev`：`-b`参数表示创建并切换，相当于以下两条命令：
  - `git branch dev`：创建分支
  - `git checkout dev` ：切换分支
- `git branch`：查看当前分支
- `git branch -a`：列出所有本地和远程的分支
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
- `git merge --no-ff -m "使用--no-ff方式合并" dev` : git 在 merge 合并的时候会生成一个新的commit，这样从分支历史上就可以看出分支信息

## bug分支
有了bug之后，可以通过一个新的临时分支来修复，修复完成后，合并分支，然后将临时分支删除。

dev分支进行一半，此时来了一个bug急需解决，此时可以使用`stash`功能（可以把当前工作现场储藏起来，等以后恢复现场后继续工作）:
1. `git stash`：把当前工作现场存储起来
2.假如需要在master上修复，那就从master分支上建立一个临时分支
  -  `git checkout master`
  -  `gitcheckout -b issue-101`
3. 手动修改bug，然后提交
   - `git add readme.txt`
   - `git commit -m "fix bug 101"`
4. 修复完成后。切换到master分支，并完成合并，最后删除issue-101分支
   - `git checkout master`
   - `git merge --no-ff -m "merged bug fix 101" issue-101`
   - `git checkout -d issue-101`
5. 修复完成后，回到dev分支继续工作
  - `git checkout dev`
  - `git stash list`：查看刚刚存储的工作现场
6. 恢复工作现场，有两种方式：
   1. `git stash pop`：恢复的同时也把stash的内容删除了
   2. `git stash apply` & `git stash drop` : 先用apply恢复，然后再用drop删除
**备注：可以多次stash，恢复的时候，先用 `git stash` 查看，然后恢复指定的stash：`git stash apply stash@{0}`**

## Feature 分支
**每添加⼀个新功能，最好新建⼀个feature分⽀**，在上⾯开发，完成后，合并，最后，删除该feature分⽀
- ` git checkout -b feature-vulcan`
- 进行正常开发
- `git add vulcan.py`
- `git commit -m "add feature vulcan"`
- `git checkout dev`
- ⼀切顺利的话，feature分⽀和bug分⽀是类似的，合并，然后删除。
  - `git merge feature-vulcan`
  - `git branch -d feature-vulcan`
- 不顺利的情况：就在此时，接到上级命令，因经费不⾜，新功能必须取消（这个分⽀必须就地销毁）**删除之前需要先切换分支**
  - feature-vulcan分⽀还没有被合并，`git branch -d feature-vulcan`会删除失败
  - 此时需要强行删除：`git branch -D feature-vulcan`

## 多人协作
- `git remote`：查看远程库信息
- `git remote -v`：查看更详细的信息

## 推送分支
- `git push origin master`：将本地 master 分支的提交推送到远程仓库 origin 的 master 分支（origin 是远程仓库的名称（通常是默认名称））
- `git push origin master:dev`：本地 master 分支推送到远程 dev 分支
- `git push origin dev`：将本地 dev 分支的更改推送到远程仓库 origin 的 dev 分支

## 抓取分⽀
- `git clone git@github.com:99night66/git_study.git`：克隆代码
- `cd git_study`：切换文件夹地址 
- `git branch`：查看本地分支（默认情况下，只能看到本地的master分⽀）  
- `git branch -r`：查看远程分支
- `git feach origin`：从远程仓库获取所有更新的分支和标签，但不会自动合并任何内容到本地分支
- `git checkout -b dev origin/dev`：创建远程origin的dev分⽀到本地(远程 dev 分支必须存在) 
- 然后正常修改文件提交推送文件。。。假如此时，还有其他人也对同一个文件做了修改，导致git push报错
- 解决方案：先⽤git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送
  - `git branch --set-upstream-to=origin/dev dev`：在本地创建了一个名为 dev 的分支，但它并没有跟踪远程的 origin/dev 分支。可以使用 --set-upstream 来设置这个跟踪关系。设置了上游分支后，可以只用 git pull 或 git push，Git 会自动将更改推送到 origin/dev 或从 origin/dev 拉取更新，而无需每次都指定远程分支
  - `git pull`
  - 手动解决冲突：解决的⽅法和分⽀管理中的解决冲突完全⼀样
  - 解决后，提交，再push

## 标签管理
### 创建标签
- 切换到需要打标签的分支上：`git checkout master`
- 打标签：`git tag v1.0`    
- 查看所有标签：`git tag`(标签不是按时间顺序列出，而是按字母排序的)
- 默认标签是打在最新提交的`commit`上的（标签（tag）在提交（commit）之后创建，用于给特定的提交打上标记）
- 如果忘记打标签：
  - 先找到历史提交的commit id：`git log --pretty=oneline --abbrev-commit`
  - 给特定commit打标签：`git tag 版本号 COMMITID`（例如：`git tag v0.9 4ccd355`）
- 查看标签信息：`git show TAGNAME`(例如：`git show v0.9`)
- 创建带有说明的标签（-a指定标签名，-m指定文字）：`git tag -a v0.1  -m "version 0.1 released" e17b470`
- 通过-s用私钥签名一个标签：`git tag -s v0.2 -m "signed version 0.2 released" f611ef7`（**签名采用PGP签名，因此，必须先要安装gpg（GnuPG）**）

### 操作标签
- 删除标签(没有推到远程时)：`git tag -d v0.1`
- 推送标签到远程仓库：`git push origin v1.0`
- 一次性推送全部尚未推动到远程的本地标签：`git push origin --tags`
- 删除标签（标签已经被推送到远程时）:
  1. 删除本地标签：`git tag -d v0.9`
  2. 删除远程标签：`git push origin :refs/tags/v0.9`
  3. 查看远程标签：`git ls-remote --tags origin`

## 使用 GitHub
### 参与开源项目（以 bootstrap 为例）
1. 访问项目主页 `https://github.com/twbs/bootstrap`
2. 点击`Fork`按钮（会自动在自己的账号下克隆一个 bootstrap 仓库）
3. clone 自己账号下的 bootstrap 仓库 `git clone git@github.com:99night66/bootstrap.git`
4. 修改代码，并推送到自己的仓库
5. 希望bootstrap官方库接收你的修改,那你就可以在github上发起一个`pull request`

### 自定义 git
- 让 git 显示颜色 ：`git config --global color.ui true`

### 忽略特殊文件
1. 在 git 工作区根目录创建 .gitignore 文件,并提交
2. 把要忽略的文件名填进去(如果文件已被 Git 跟踪，即使在 .gitignore 中，它们仍会在 git status 中显示。只有未被跟踪的文件，才会遵循 .gitignore 的规则。)
3. 停止跟踪文件：`git rm --cached <file>`
4. 提交更改：`git commit -m "Stop tracking ignored files"`

### 配置别名
例如：
- status用st代替：`git config --global alias.st status`,(--global 是全局参数，也就是这些命令在这台电脑的所有git仓库下都有用)
- checkout用co代替：`git config --global alias.co checkout`
- commit用ci代替：`git config --global alias.ci commit`
- branch用br代替：`git config --global alias.br branch`
- reset HEAD用unstge代替：`git config --global alias.unstage 'reset HEAD' `
