# Git

### 总览

![](D:\BNA\note\imgs\18087435-2b52aaf65be47442.webp)

![](D:\BNA\note\imgs\18087435-bf2a996ef50a21b0.webp)



### 详解

#### `git add`

* `git add -u`：将文件的修改、文件的删除，添加到暂存区。
* `git add .`：将文件的修改，文件的新建，添加到暂存区。
* `git add -A`：将文件的修改，文件的删除，文件的新建，添加到暂存区。



### 示例

创建分支：`$ git branch mybranch`

切换分支：`$ git checkout mybranch`

创建并切换分支：`$ git checkout -b mybranch`

更新master主线上的东西到该分支上：`$ git rebase master`

切换到master分支：`$ git checkout master`

更新mybranch分支上的东西到master上：`$ git rebase mybranch`

提交：`git commit -a`

对最近一次commit的进行修改：`git commit -a -amend`

