# git初学

[学习参考网站](https://www.liaoxuefeng.com/wiki/896043488029600)

## 一、本地git操作

打开文件夹执行 `git init` **初始化**该文件夹  
`ls -a`查看文件  
`git clone [url]` 或 `git clone [url] [directory]` 克隆远程库到本地文件夹  

```git
git clone git@github.com:fsliurujie/test.git         --SSH协议
git clone git://github.com/fsliurujie/test.git          --GIT协议
git clone https://github.com/fsliurujie/test.git      --HTTPS协议
```

添加文件到暂存区 `git add 文件1 文件2...`  
`git status -s` 和 `git status` 查看项目当前状态  
提交到仓库 `git commiit -m "这是第一次提交"`  
查看文件做过哪些修改`git diff "文件名"`  
`git log` 从最近到最远的查看提交日志，增加参数 `git log --pretty=online`  
`git log --graph --pretty=online --abbrev-commit`  
回退到上一个或几个版本 `git reset --hard HEAD^` 其中HEAD是当前版本，HEAD^是上一个版本，HEAD^^是上上版本……，HEAD~100回退到上一百个版本  
查看记录的每次命令 `git reflog`  

### 名词解释

**工作区（Working Directory）**：当前目录  
**版本库（Repository）**：工作区的隐藏目录`.git`就是Git版本库  
Git的版本库储存了一些东西，最终要的是stage（或index）的暂存区，还有Git自动创建的第一个分支`master`，以及指向master的第一个指针`HEAD`  
把文件的提交分为两步，  

- `git add`把文件添加进去，实际上是把文件修改添加到暂存区
- `git commit`是把暂存区的所有内容提交到master分支上  

![0.jfif](./img/0.jfif "工作区和版本库")  

### Git管理的是修改而非文件  

比较当前工作区文件与版本库中的文件：  
 `git diff HEAD -- "文件名"`  
丢弃工作区的修改，回到最近 `git commit` 或 `git add` 的状态使用命令：  
 `git checkout -- 文件名` 或 `git restore 文件名`  
对于修改且 `git add` 到暂存区但未提交，要撤销暂存区的修改将修改放回工作区使用命令：  
 `git reset HEAD 文件名`  或 `git restore --staged 文件名`  

### 删除文件  

在工作区删除文件后，Git知道你删除了该文件，且Git的版本库还存有该文件  

- 若确实要删除该文件，则需要 `git rm` 且 `git commit`：  
 `git rm 文件名`  
 `git commit -m "说明"`  
- 若要撤销误删则使用命令：  
 `git checkout -- 文件名`  
**`git checkout` 其实是用版本库中的版本替换工作区的版本（checkout检查点）无论是修改还是删除都能“一键还原”**  

### 添加远程库  

- 与远程库相关联：  
`git remote add origin [url]`  
- 推送到远程仓库：
`git push origin master`  
若相关联失败先使用命令 `git remote rm origin`  

### 创建与合并分支  

- **创建分支**：  
`git branch 分支名`  
- **切换分支**：  
`git checkout 分支名` 或`git switch 分支名`  
相当于创建并切换分支命令：  
`git checkout -b 分支名` 或`git switch -c 分支名`  
- **查看分支**：  
`git branch`  
在一个分支上修改了数据后，可以合并分支  
- **合并另一分支到当前分支**：  
`git merge 分支名`  
- **删除分支**：  
`git branch -d 分支名`  

***图解分支原理：***  
初始时的master分支  
![初始时的master分支](./img/master.png "master分支")  
添加了dev分支  
![添加了dev分支](./img/dev.png "添加dev分支")  
切换到dev分支并对文件修改  
![切换到dev分支并对文件修改](./img/dev修改.png "dev分支修改文件")  
合并分支  
![合并分支](./img/合并.png "合并")  
删除分支  
![删除分支](./img/删除.png "删除")  

### 解决冲突  

当在开启后的新分支做修改，且原有分支也做修改，则两个分支合并就会发生冲突  
![冲突](./img/冲突.png "冲突")  
冲突后，在冲突的文本会被标记，可以手动修改然后提交来解决冲突  
![解决冲突](./img/解决冲突.png "解决冲突")  

### 分支管理  

通常，在合并分支时，Git可能会采用 `Fast forward` 模式，在这种模式下删除分支，会丢失分支信息。若强行禁用 `Fast forward` ，Git就会在merge时创建一个新的commit，如此，从分支历史就能看出分支信息  
在创建新分支并修改文件提交后，使用 `--no-ff` 方式的 `git merge`  
![merge](./img/分支管理.png "分支管理")  
团队合作  
![合作](./img/团队合作.png "团队合作")  

### BUG分支  

当正在当前分支操作时，遇到新的任务（修复bug），可以保存当前分支的工作现场：  
`git stash`  
在哪个分支上修复则从哪个分支上新建一个临时分支，然后可以使用 `--no-ff` 的 `git merge` 合并到主分支上  
列举保存的工作现场：  
`git stash list`  
返回工作现场先 `git stash apply` 恢复后，stash的内容未删除，再使用 `git stash drop`来删除。或使用 `git stash pop` 恢复的同时也删除stash的内容  
在master上修复后，同样的bug也存在与其他分支如dev上，将修改复制到其他分支上使用：  
`git cherry-pick <commit>` commit为提交号  

### 丢弃未合并分支  

开发一个新功能，最好新建一个分支  
对于未合并的分支，要强行删除使用命令：  
`git branch -D <name>`  

### 查看远程仓库信息  

`git remote`  
显示更详细信息：`git remote -v`  

### 推送分支  

推送分支就是将分支的所有修改推送到远程仓库  
`git push origin 分支名`  

### 多人协作  

在本地创建和远程分支对应的分支：  
`git checkout -b branch-name origin/branch-name`  
建立本地分支和远程分支的关联，使用：  
`git branch --set-upstream branch-name origin/branch-name`  

### rebase  

rebase操作可以把本地未push的的分叉提交历史整理成直线  
`git rebase`  

---

## 二、标签

- **创建标签**：  
`git tag 标签` 或 `git tag 标签 commit号`  
- **创建有说明的标签**：  
`git tag -a 标签 -m "说明" commit号`  
- **查看标签**：  
`git show 标签`  
- **查看所有标签**：  
`git tag`  
- **删除标签**：  
`git tag -d 标签`  
- **推送标签到远程**：  
`git push origin 标签` 推送某标签或 `git push origin --tag` 推送全部  
**标签已推送到远程，要删除标签，** 需要先删除本地标签，再删除远程标签  
- **删除远程仓库分支标签**：  
`git push origin :refs/tags/标签名`  

## 远程库关联  

如 `git remote add github [url]` 和 `git remote add gitee [url]`  
分别关联github和码云  
查看关联：  
`git remote`  

## 三、自定义git  

### 忽略特殊文件  

在Git工作区的根目录下创建一个 `.gitignore`  
把要忽略文件名写在里面  
强制添加写入.gitignore中的文件，到git中： `git add -f 文件名`  
查看忽略规则与添加行为冲突：`git check-ignore -v 文件名`  

### 配置别名  

为命令的一部分简写配置别名：  
`git config --global alias.简写 原名或"原名"`  
加--global是全局有效，否则只在当前仓库有效  
查看全局配置文件在 用户主目录下的 `.gitconfig` 中  
