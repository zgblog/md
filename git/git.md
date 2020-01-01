# git初学

## 一、本地git操作

打开文件夹执行 `git init` **初始化**该文件夹  
`ls -a`查看文件  
`git clone [url]` 或 `git clone [url] [directory]` 克隆到本地文件夹  

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
回退到上一个或几个版本 `git reset --hard HEAD^` 其中HEAD是当前版本，HEAD^是上一个版本，HEAD^^是上上版本……，HEAD~100回退到上一百个版本  
查看记录的每次命令 `git reflog`  

### 名词解释

**工作区（Working Directory）**：当前目录  
**版本库（Repository）**：工作区的隐藏目录`.git`就是Git版本库  
Git的版本库储存了一些东西，最终要的是stage（或index）的暂存区，还有Git自动创建的第一个分支`master`，以及指向master的第一个指针`HEAD`  
把文件的提交分为两步，  

- `git add`把文件添加进去，实际上是把文件修改添加到暂存区
- `git commit`是把暂存区的所有内容提交到master分支上  

![0.jfif](./0.jfif "工作区和版本库")  

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
