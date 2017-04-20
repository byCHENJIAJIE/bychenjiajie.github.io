---
author: kresnikwang
comments: true
date: 2015-04-28 17:42:32+00:00
layout: post
title: PHP, Angular JS Development|My Export Quote|农产品出口工具开发
categories:
- Works
- Tech
tags:
- bootstrap
- javascript
- php
- AngularJS
---

# GIT学习笔记

标签（空格分隔）： 未分类

---

[toc]
## Git命令
### git config --listgit config --global  user.name/email
配置全局用户名/邮箱
### git config --list
查看所有配置项
### git init
```shell
可以用以下linux命令新建一个文件，当然也可以到文件管理器里手动新建：
$ mkdir learngit//新建文件
$ cd learngit//打开文件
$ pwd//显示当前目录
/Users/michael/learngit
```
进入需要新建repo的文件夹，执行下面命令
```shell
$ git init
```
在本地新建一个repo,进入一个项目目录,执行`git init`,会初始化一个repo,并在当前文件夹下创建一个.git文件夹.
### git clone
获取一个url对应的远程Git repo, 创建一个local copy.

一般的格式是`git clone [url]`.

clone下来的repo会以url最后一个斜线后面的名称命名,创建一个文件夹,如果想要指定特定的名称,可以`git clone [url] newname`指定.
### git status
列出当前目录所有还没有被git管理的文件和被git管理且被修改但还未提交(git commit)的文件

`git status -uno`可以只列出所有已经被git管理的且被修改但没提交的文件
`git status -s` -s表示short, -s的输出标记会有两列,第一列是对staging区域而言,第二列是对working目录而言.
### git add
在提交之前,Git有一个暂存区(staging area),可以放入新添加的文件或者加入新的改动. commit时提交的改动是上一次加入到staging area中的改动,而不是我们disk上的改动.

`git add . `会递归地添加当前工作目录中的所有文件.
### git commit
提交已经被add进来的改动.

`git commit -m '提交说明'`

`git commit -a` 会先把所有已经track的文件的改动add进来,然后提交(有点像svn的一次提交,不用先暂存). 对于没有track的文件,还是需要git add一下.
可以-a的同时可以-m：
```shell
$ git commit -a -m '提交说明'
```


`git commit --amend `有时候我们提交完了才发现漏掉了几个文件没有加，或者提交信息写错了。想要撤消刚才的提交操作，可以使用 --amend 选项重新提交：
```shell
$ git commit --amend
```
此命令将使用当前的暂存区域快照提交。如果刚才提交完没有作任何改动，直接运行此命令的话，相当于有机会重新编辑提交说明，但将要提交的文件快照和之前的一样。

启动文本编辑器后，会看到上次提交时的说明，编辑它确认没问题后保存退出，就会使用新的提交说明覆盖刚才失误的提交。

如果刚才提交时忘了暂存某些修改，可以先补上暂存操作，然后再运行 --amend 提交：
```shell
$ git commit -m '提交说明'
$ git add forgotten_file
$ git commit --amend
```
上面的三条命令最终只是产生一个提交，第二个提交命令修正了第一个的提交内容。
### git log
show commit history of a branch.

`git log --oneline --number`: 每条log只显示一行,显示number条.

`git log --oneline --graph`:可以图形化地表示出分支合并历史.

`git log branchname`可以显示特定分支的log.

`git log --oneline branch1 ^branch2`可以查看在分支1,却不在分支2中的提交.^表示排除这个分支(Window下可能要给^branch2加上引号).

`git log --decorate`会显示出tag信息.

`git log --author=[author name]` 可以指定作者的提交历史.

`git log --since --before --until --after` 根据提交时间筛选log.
`--no-merges`可以将merge的commits排除在外.

`git log --grep` 根据commit信息过滤log
> git log --grep=keywords
默认情况下, git log --grep --author是OR的关系,即满足一条即被返回,如果你想让它们是AND的关系,可以加上--all-match的option.

`git log -S`: filter by introduced diff.
> 比如: git log -SmethodName (注意S和后面的词之间没有等号分隔).

`git log -p`: show patch introduced at each commit.
> 每一个提交都是一个快照(snapshot),Git会把每次提交的diff计算出来,作为一个patch显示给你看.
另一种方法是git show [SHA].

`git log --stat`: show diffstat of changes introduced at each commit.
同样是用来看改动的相对信息的,--stat比-p的输出更简单一些.
### 对比 git diff
不加参数的`$ git diff`:
此命令比较的是工作目录中当前文件和暂存区域快照之间的差异,也就是修改之后还没有暂存起来的变化内容.（**工作区和暂存区**之间的差异）

若要看已经暂存起来的文件和上次提交时的快照之间的差异（**暂存区和版本库**）,可以用:
```shell
$ git diff --cached
```
(Git 1.6.1 及更高版本还允许使用 `git diff --staged`，效果是相同的).

比较woking directory和上次提交之间所有的改动（**工作区和版本库**）,可以用：
```shell
//效果一样：
$ git diff 分支名
$ git diff HEAD
```
如果想比较两个分支的差异，可以用：
```shell
$ git diff 分支1..分支2
# 或者
$ git diff 分支1 分支2

$ git diff master..test
```
上面第五行这条命令只显示两个分支间的差异，如果你想找出‘master’,‘test’的共有 父分支和'test'分支之间的差异，你用3个‘.'来取代前面的两个'.' 。
```shell
$ git diff master...test
```
如果想看自从某个版本之后都改动了什么,可以用:
```shell
$ git diff [version tag]
```
跟log命令一样,diff也可以加上--stat参数来简化输出.

### 撤销
```shell
$ git reset HEAD
```
这个命令用来把不小心add进去的文件从staged状态取出来（**暂存区退回工作区**）,可以**单独针对**某一个文件操作: `git reset HEAD - - filename`, 这个- - 也可以不加.
这里的HEAD可以被写成任何一次提交的SHA-1(commit_id).

------------


```shell
$ git checkout -- filename
```
把文件在工作区的修改全部撤销，这里有**两种**情况：
1. 一种是文件自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
2. 一种是文件已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件**回到最近一次**`git commit`或`git add`时的状态。

`git checkout -- file`命令中的`--`**很重要**，没有`--`，就变成了“切换到另一个分支”的命令;

------------


有时候我们提交完了才发现漏掉了几个文件没有加，或者提交信息写错了。想要撤消刚才的提交操作，可以使用 --amend 选项**重新提交**（同时可以备注）：
```shell
$ git commit - m "提交说明"--amend
```
此命令将**使用当前的暂存区域快照提交**，有两种情况：
1. 如果刚才提交完没有作任何改动，直接运行此命令的话，相当于有机会**重新编辑提交说明**，但将要提交的文件快照和之前的一样。
2. 如果刚才提交时忘了暂存某些修改，可以**先补上暂存操作**，然后再运行 `--amend` 提交
```shell
$ git commit -m '提交说明'
$ git add filename
$ git commit --amend
```
上面的三条命令最终**只是产生一个提交**，第二个提交命令修正了第一个的提交内容。

### 恢复
误删误操作后想恢复到之前的版本可以用下面的方法：
```shell
git checkout commit_id filename
```
这个方法可以恢复到**对应版本的指定文件**；

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

------------

如果想要把**对应版本的所有文件**都恢复，用下面的方法：
```shell
$ git reset --hard commit_id
```
也可以通过**操作HEAD的指向**来实现：
```shell
$ git reset --hard HEAD^//退回到上一个版本
$ git reset --hard HEAD^^//退回到上上一个版本
$ git reset --hard HEAD~3//退回到上三个版本
$ git reset --hard HEAD~4//退回到上四个版本
```
退回以前的版本后，`git log`里只能看到当前版本往前的版本了，但是如果想**恢复当前版本往后的版本**呢？可以用下面的方法查看所有提交过的版本`commit_id`:
```shell
$ git reflog
```
### 删除git rm
`git rm filename`：当在工作区删除了文件时，执行这句可以把对应在暂存区的文件删除.
> **注意**：`$ rm filename`前面没有git，就等于直接在文件管理器中把文件删了

`git rm -f filename`: 从staging区移除文件,同时也移除出工作目录.

`git rm --cached filename`: 从staging区移除文件,但留在工作目录中.

`git rm --cached`从功能上等同于`git reset HEAD`,清除了缓存区,但不动工作目录树
### 同步到远程仓库
配置好后：

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支master推送到远程。
```shell
$ git push <远程主机名> <本地分支名>:<远程分支名>
```
注意，**分支推送顺序的写法是<来源地>:<目的地>**，所以`git pull`是<远程分支>:<本地分支>，而`git push`是<本地分支>:<远程分支>。

如果**省略远程分支名**，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，**则会被新建**。
```shell
$ git push origin master
```
上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。

如果省略本地分支名，则表示**删除指定的远程分支**，因为这等同于推送一个空的本地分支到远程分支。
```shell
$ git push origin :master
# 等同于
$ git push origin --delete master
```
如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。
```shell
$ git push origin
```
如果当前分支只有一个追踪分支，那么主机名都可以省略。
```shell
$ git push
```
如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用`git push`
```shell
$ git push -u origin master
```
上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用`git push`了。

不带任何参数的`git push`，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用`git config`命令。
```shell
$ git config --global push.default matching
# 或者
$ git config --global push.default simple
```
还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用`--all`选项。
```shell
$ git push --all origin
```
上面命令表示，将所有本地分支都推送到origin主机。

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做`git pull`合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用`--force`选项。
```shell
$ git push --force origin 
```
上面命令使用`--force`选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用`--force`选项。

最后，git push不会推送标签（tag），除非使用`--tags`选项。
```shell
$ git push origin --tags
```

------------


用`$ git remote`可以列出已经存在的远程分支；

带参数`-v`列出详细信息，在每一个名字后面列出其远程url；

`$ git remote add [shortname] [url]`可以添加一个新的远程仓库,可以指定一个简单的名字,以便将来引用；

------------


Git中从远程的分支**获取最新的版本到本地**有2个命令：
1. `git fetch`相当于是从远程获取最新版本到本地，不会自动merge;
2. `git pull`命令是取回远程主机某个分支的更新，再与本地的指定分支合并;

```shell
$ git fetch <远程主机名>
```
上面命令将某个远程主机的更新，全部取回本地;

`git fetch`命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响。
默认情况下，`git fetch`取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。
```shell
$ git fetch <远程主机名> <分支名>
```
所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用`origin/master`读取。

`git branch`命令的`-r`选项，可以用来查看远程分支，`-a`选项查看所有分支。
```shell
$ git branch -r
origin/master

$ git branch -a
* master
  remotes/origin/master
```
上面命令表示，本地主机的当前分支是`master`，远程分支是`origin/master`。
取回远程主机的更新以后，可以在它的基础上，使用`git checkout`命令创建一个新的分支。
```shell
$ git checkout -b newBrach origin/master
```
上面命令表示，在`origin/master`的基础上，创建一个新分支。
此外，也可以使用`git merge`命令或者`git rebase`命令，在本地分支上合并远程分支
```shell
$ git merge origin/master
# 或者
$ git rebase origin/master
```
上面命令表示在当前分支上，合并`origin/master`。
```shell
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```
如果远程分支是与当前分支合并，则冒号后面的部分可以省略。
**实质上**，这等同于先做git fetch，再做git merge。

------------

在某些场合，Git会自动在本地分支与远程分支之间，建立一种**追踪关系**（tracking）。比如，在`git clone`的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动"追踪"origin/master分支。

Git也允许手动建立追踪关系。
```shell
$ git branch --set-upstream master origin/next
```
上面命令指定`master`分支追踪`origin/next`分支。
如果当前分支与远程分支存在追踪关系，git pull就可以**省略远程分支名**。
```shell
$ git pull origin
```
上面命令表示，本地的**当前分支**自动与对应的origin主机"**追踪分支**"（remote-tracking branch）进行合并。
如果当前分支只有一个追踪分支，连远程主机名都可以省略。
```shell
$ git pull
```
上面命令表示，当前分支自动与唯一一个追踪分支进行合并。

如果合并需要采用rebase模式，可以使用`--rebase`选项。
```shell
$ git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
```
如果远程主机删除了某个分支，默认情况下，`git pull` 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致`git pull`不知不觉删除了本地分支。

但是，你可以改变这个行为，加上参数 `-p` 就会在本地删除远程已经删除的分支。
```shell
$ git pull -p
# 等同于下面的命令
$ git fetch --prune origin 
$ git fetch -p
```
### 分支
`git branch`不带参数：列出本地已经存在的分支，并且在当前分支的前面加“*”号标记

`git branch -r `列出远程分支

`git branch -a` 列出本地分支和远程分支

`git branch branchname`创建一个新的本地分支，需要注意，此处只是创建分支，**不进行分支切换**

`git branch -m oldbranch newbranch` 重命名分支，如果newbranch名字分支已经存在，则需要使用`-M`强制重命名，否则，使用`-m`进行重命名

`git branch -d branchname `删除branchname分支,`-M`强制删除
一般分支合并到master后就可以删除掉；
`git branch -d -r branchname` 删除远程branchname分支

创建了一个新分支，需要用`git checkout branchname`切换到新分支，当然也可以用`git checkout -b branchName`命令**同时**创建和切换；

最后可以用`git merge`进行合并分支；
如果两个分支修改了同一个文件的同一块区域，GIT会报告**内容冲突**；
这时应该先编辑冲突，然后`git commit -a`提交**或者**运行` git add` 将把它们标记为已解决状态（实际上就是来一次快照保存到暂存区域）。因为一旦暂存，就表示冲突已经解决。
注：对于git来讲，编辑冲突跟平时的修改代码没什么差异。修改完成后，都是要把修改添加到缓存，然后commit。

如果想看一下哪些分支已经（或尚未）与当前分支合并的分支，可以用` –merge` 和` –no-merged` 参数



