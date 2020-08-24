# Git

### 总览

![](D:\BNA\note\imgs\18087435-2b52aaf65be47442.webp)

![](D:\BNA\note\imgs\18087435-bf2a996ef50a21b0.webp)

### Git引用初识（HEAD、分支、tag）

#### 引用原理

`引用` 指的是对  `提交记录` 的引用

`提交记录` 用 `哈希值` 唯一标识

每个 `引用` 用一个文件表示，文件中保存 `其引用的提交记录的哈希值`



#### 引用分类

* ##### 分支

  * 可变，在不同的时刻可以指向 `不同的提交记录`
  * 本地分支
    * 对应  `.git/refs/heads/目录` 中的文件
    * 每个 `本地仓库` 有多个 `本地分支`
  * 远程分支
    * 对应 `.git/refs/remotes/<远端仓库名>/目录` 中的文件
    * 每个 `本地仓库` 可以对应多个 `远端仓库` ，同时每个 `远端仓库` 可以有多个 `远端分支`

* ##### tag

  * 对应 `.git/refs/tags/目录` 中的文件
  * 不可变，除非删除后重新创建，否则总是指向 `特定的提交记录`
  * 每个 git 仓库可以有多个 `tag`

* ##### HEAD

  * 对应 `.git/HEAD` 文件
  * 可变
    * 通常指向某个 `本地分支` ，即引用的引用
    * 也可以直接指向 `某个提交记录` ，成为 `HEAD detached` ，即分离头指针状态
    * 也可以指向 `tag` ，git将这种情况也处理成 `HEAD detached`
    * 也可以指向 `远端分支` ，git将这种情况也处理成 `HEAD detached`
  * 每个 git 仓库只用一个 `HEAD`
  * 表示当前 `工作区` 检出的文件（或者说文件在修改之前）是属于哪个 `提交记录` 的
  * `git checkout 指令` ，就是在改变HEAD的指向
    * `git checkout 本地分支名`
    * `git checkout 提交记录哈希值` ，detached
    * `git checkout 远端分支名` ，detached
    * `git checkouut tag名` ，detached



#### 实验

```shell
$ git checkout master
Switched to branch 'master'

$ cat .git/HEAD
ref: refs/heads/master

$ cat .git/refs/heads/master
89d496d44f93d107a7eb404890cd15a14ba8845d
```

checkout master 后， HEAD 指向 master ， master 指向 89d496



```shell
$ git checkout milestone
Note: checking out 'milestone'.
You are in 'detached HEAD' state.
HEAD is now at eecc5fe milestone

$ cat .git/refs/tags/milestone
eecc5fe060e5b86957f931fd931beae4f206d4eb

$ cat .git/HEAD
eecc5fe060e5b86957f931fd931beae4f206d4eb
```

checkout tag milestone 后， HEAD 指向 eecc5f ， detached HEAD



### Git 中的 'HEAD' 是什么？-  Git 名词解释

#### 问题来源

git 恢复文件到初始状态的命令：

```shell
$ git reset HEAD <file>
```

git 展示提交日志命令：

```shell
$ git log
commit c4f9d71863ab78cfca754c78e9f0f2bf66a2bd77 (HEAD -> master)
```

在这些命令中常常会看到 `HEAD` 这个名词，它指的是什么呢？



#### 回答

这要从 git 的分支说起， git 中的分支，其实本质上仅仅是个指向 commit 对象的可变指针。 git 是如何知道你当前在哪个分支上工作的呢？

其实答案也很简单，它保存着一个名为 HEAD 的特别指针。在 git 中，它是一个指向你正在工作中的本地分支的指针，可以将 HEAD 想象为当前分支的别名。

![](D:\BNA\note\imgs\14340223-46fb90f6af8af539.webp)

​                                                                                HEAD 指向当前所在的分支 —— master

------

所以，

* `git reset HEAD <file>` 指的是恢复到当前分支中文件的状态。
* `git log` 日志展示中 `HEAD -> master` 指的是：当前分支指向的是 master 分支



### Git Tag作用

#### 官方解释

Git tag 给当前分支

#### 个人解释

其实道理和 commit 的 commit -sha1 有些相似，其实就是给当前的版本做个标记，以便回退到此版本。如果使用 commit -sha1 ，大家都记不住那条冗长的 sha1 码，所以用 tag 标签来做记录

发布一个版本时，我们通常先在版本库中打一个标签（tag）

#### 详解

1. `git tag <tagname>` 就可以打一个新标签

   标签都只存储在本地，不会自动推送到远程

2. 可以用命令 `git tag` 查看所有标签

3. 在历史提交上打 tag

   查看 commit id ，使用 commit id 打 tag：

   `git tag <tagname> <commitid>`

   注意，标签不是按时间顺序列出，而是按字母排序的。可以用 `git show <tagname>` 查看标签信息

4. 带有说明的标签，用 -a 指定标签名， -m 指定说明文字：

   `git tag -a <tagname> -m <message> <commitid>`

5. 删除标签：

   `git tag -d <tagname>`

6. 推送某个标签到远程：

   `git push origin <tagname>`

7. 一次性推送全部尚未推送到远程的本地标签：

   `git push origin --tags`



### `git merge` 和 `git merge --no-ff` 的区别

#### merge的三种状态

1. #####  --ff (fast-forward)

   Git 合并两个分支时，如果顺着一个分支走下去可以到达另一个分支的话，那么 Git 在合并两者时，只会简单地把指针右移，叫做“快进”（fast-forward）。不过这种情况如果删除分支，则会丢失 merge 分支信息。

2. #####  --squash

   把一些不必要 commit 进行压缩，比如说，你的 feature 在开发的时候写的 commit 很乱，那么我们合并的时候不希望把这些历史 commit 带过来，于是使用 --squash 进行合并，此时文件已经同合并后一样了，但不移动 HEAD ，不提交。需要进行一次额外的 commit 来“总结”一下，然后完成最终的合并。

3. ##### --no-ff

   关闭 fast-forward 模式，在提交的时候，会创建一个 merge 的 commit 信息，然后合并的和 master 分支。

   merge 的不同行为，向后看，其实最终都会将代码合并到 master 分支，而区别仅仅只是分支上的简洁清晰的问题。然后向前看，也就是我们使用 reset 的时候，就会发现，不同的行为就带来了不同的影响。



### `git reset` 的用法

`git reset [<mode>] [<commit>]`

git reset 命令可以将当前的 HEAD 重置到特定的状态。

首先要搞清楚下面几个概念：

*  HEAD ：HEAD 就是指向当前分支当前版本的游标
*  Index ：Index 即为暂存区，当你修改了你的 git 仓库里的一个文件时，这些变化一开始是 unstaged 状态，为了提交这些修改，你需要使用 git add 把它加入到 index ，使它成为 staged 状态。当你提交一个 commit 时，index 里面的修改被提交。
*  working tree ：即当前的工作目录。

git reset 将当前分支的 HEAD 指向给定的版本，并根据模式的不同决定是否修改 index 和 working tree。

常用的有三种模式，--soft ， --mixed ，--hard 。如果没有给出 \<mode\> 则默认是 --mixed 。

#### --soft

使用 --soft 参数将会仅仅重置 HEAD 到指定的版本，不会修改 index 和 working tree。

#### --mixed

使用 --mixed 参数与 --soft 的不同之处在于，--mixed 修改了 index ，使其与第二个版本匹配。index 中给定 commit 之后的修改被 unstaged。

#### --hard

使用 --hard 同时也会修改 working tree ，也就是当前的工作目录，如果我们执行 `git reset --hard HEAD~` ，那么最后一次提交的修改，包括本地文件的修改都会被清除，彻底还原到上一次提交的状态且无法找回。所以在执行 reset --hard 之前一定要小心。



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



### 场景

1. 针对当前项目

   * `git add -A`

   * `git commit -m "finish the project"`

2. 此时尴尬的地方出现了，你发现了项目中某个地方有个bug，这可怎么办

   * 修个项目中的bug
   * `git add bugFileName`
   * `git commit -m "fix: bug"`

   这种方式是一种解决方法，但是提交到远程仓库，git log日志里面就会多很多无效的fix日志信息，不美观，这个时候就要使用 --amend，保留上次的commit提交日志

3. `--amend --no-edit`

   * 修改项目中的bug
   * `git add bugFileName`
   * `git commit --amend --no-edit`会保留上次的提交信息
   * `git push --no-thin origin HEAD:refs/for/master`重新push到远端服务器