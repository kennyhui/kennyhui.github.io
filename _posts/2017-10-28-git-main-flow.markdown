---
layout:     post
title:      "Git工作流指南"
subtitle:   "集中式工作流"
date:        2017-10-27 10:00:00
author:     ""
header-img: "img/post-bg-2015.jpg"
tags:
   - git, 工作流
---


转到分布式版本控制系统看起来像个令人生畏的任务，但不改变已用的工作流你也可以用上Git带来的收益。团队可以用和Subversion完全不变的方式来开发项目。


![Alt text](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/git-workflow-svn.png)


但使用Git加强开发的工作流，Git比SVN有几个优势。首先，每个开发可以有属于自己的整个工程的本地拷贝。隔离的环境让各个开发者的工作和项目的其他部分（修改）独立开来 —— 即自由地提交到自己的本地仓库，先完全忽略上游的开发，直到方便的时候再把修改反馈上去。

其次，Git提供了强壮的分支和合并模型。不像SVN，Git的分支设计成可以做为一种用来在仓库之间集成代码和分享修改的『失败安全』的机制。


## 工作方式

像Subversion一样，集中式工作流以中央仓库作为项目所有修改的单点实体。相比SVN缺省的开发分支trunk，Git叫做master，所有修改提交到这个分支上。该工作流只用到master这一个分支。

开发者开始先克隆中央仓库。在自己的项目拷贝中，像SVN一样的编辑文件和提交修改；但修改是存在本地的，和中央仓库是完全隔离的。开发者可以把和上游的同步延后到一个方便时间点。

要发布修改到正式项目中，开发者要把本地master分支的修改『推（push）』到中央仓库中。这相当于svn commit操作，但push操作会把所有还不在中央仓库的本地提交都推上去。

## 冲突解决

中央仓库代表了正式项目，所以提交历史应该被尊重且是稳定不变的。如果开发者本地的提交历史和中央仓库有分歧，Git会拒绝push提交否则会覆盖已经在中央库的正式提交。

git-workflow-svn-managingconflicts

在开发者提交自己功能修改到中央库前，需要先fetch在中央库的新增提交，rebase自己提交到中央库提交历史之上。这样做的意思是在说，『我要把自己的修改加到别人已经完成的修改上。』最终的结果是一个完美的线性历史，就像以前的SVN的工作流中一样。

如果本地修改和上游提交有冲突，Git会暂停rebase过程，给你手动解决冲突的机会。Git解决合并冲突，用和生成提交一样的git status和git add命令，很一致方便。还有一点，如果解决冲突时遇到麻烦，Git可以很简单中止整个rebase操作，重来一次（或者让别人来帮助解决）。


## 示例

让我们一起逐步分解来看看一个常见的小团队如何用这个工作流来协作的。有两个开发者小明和小红，看他们是如何开发自己的功能并提交到中央仓库上的。

### 有人先初始化好中央仓库

第一步，有人在服务器上创建好中央仓库。如果是新项目，你可以初始化一个空仓库；否则你要导入已有的Git或SVN仓库。

中央仓库应该是个裸仓库（bare repository），即没有工作目录（working directory）的仓库。可以用下面的命令创建：

ssh user@host
git init --bare /path/to/repo.git

确保写上有效的user（SSH的用户名），host（服务器的域名或IP地址），/path/to/repo.git（你想存放仓库的位置）。注意，为了表示是一个裸仓库，按照约定加上.git扩展名到仓库名上。

## 所有人克隆中央仓库

下一步，各个开发者创建整个项目的本地拷贝。通过git clone命令完成：

git clone ssh://user@host/path/to/repo.git

基于你后续会持续和克隆的仓库做交互的假设，克隆仓库时Git会自动添加远程别名origin指回『父』仓库。


### 小明开发功能

在小明的本地仓库中，他使用标准的Git过程开发功能：编辑、暂存（Stage）和提交。如果你不熟悉暂存区（Staging Area），这里说明一下：暂存区的用来准备一个提交，但可以不用把工作目录中所有的修改内容都包含进来。这样你可以创建一个高度聚焦的提交，尽管你本地修改很多内容。

git status # 查看本地仓库的修改状态
git add # 暂存文件
git commit # 提交文件


请记住，因为这些命令生成的是本地提交，小明可以按自己需求反复操作多次，而不用担心中央仓库上有了什么操作。对需要多个更简单更原子分块的大功能，这个做法是很有用的。

### 小红开发功能

与此同时，小红在自己的本地仓库中用相同的编辑、暂存和提交过程开发功能。和小明一样，她也不关心中央仓库有没有新提交；当然更不关心小明在他的本地仓库中的操作，因为所有本地仓库都是私有的。

### 小明发布功能


一旦小明完成了他的功能开发，会发布他的本地提交到中央仓库中，这样其它团队成员可以看到他的修改。他可以用下面的git push命令：

git push origin master

注意，origin是在小明克隆仓库时Git创建的远程中央仓库别名。master参数告诉Git推送的分支。由于中央仓库自从小明克隆以来还没有被更新过，所以push操作不会有冲突，成功完成。


### 小红试着发布功能

一起来看看在小明发布修改后，小红push修改会怎么样？她使用完全一样的push命令：

git push origin master

但她的本地历史已经和中央仓库有分岐了，Git拒绝操作并给出下面很长的出错消息：


```
error: failed to push some refs to '/path/to/repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

这避免了小红覆写正式的提交。她要先pull小明的更新到她的本地仓库合并上她的本地修改后，再重试。

### 小红在小明的提交之上rebase


小红用git pull合并上游的修改到自己的仓库中。这条命令类似svn update——拉取所有上游提交命令到小红的本地仓库，并尝试和她的本地修改合并：

git pull --rebase origin master

--rebase选项告诉Git把小红的提交移到同步了中央仓库修改后的master分支的顶部，如下图所示：

![Alt text](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/git-workflow-svn-6.png)


如果你忘加了这个选项，pull操作仍然可以完成，但每次pull操作要同步中央仓库中别人修改时，提交历史会以一个多余的『合并提交』结尾。对于集中式工作流，最好是使用rebase而不是生成一个合并提交。

### 小红解决合并冲突

rebase操作过程是把本地提交一次一个地迁移到更新了的中央仓库master分支之上。这意味着可能要解决在迁移某个提交时出现的合并冲突，而不是解决包含了所有提交的大型合并时所出现的冲突。这样的方式让你尽可能保持每个提交的聚焦和项目历史的整洁。反过来，简化了哪里引入Bug的分析，如果有必要，回滚修改也可以做到对项目影响最小。

如果小红和小明的功能是相关的，不大可能在rebase过程中有冲突。如果有，Git在合并有冲突的提交处暂停rebase过程，输出下面的信息并带上相关的指令：

CONFLICT (content): Merge conflict in

[](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/git-workflow-svn-8.png)



Git很赞的一点是，任何人可以解决他自己的冲突。在这个例子中，小红可以简单的运行git status命令来查看哪里有问题。冲突文件列在Unmerged paths（未合并路径）一节中：

```

# Unmerged paths:
# (use "git reset HEAD <some-file>..." to unstage)
# (use "git add/rm <some-file>..." as appropriate to mark resolution)
#
# both modified: <some-file>

```

接着小红编辑这些文件。修改完成后，用老套路暂存这些文件，并让git rebase完成剩下的事：

git add
git rebase --continue

要做的就这些了。Git会继续一个一个地合并后面的提交，如其它的提交有冲突就重复这个过程。

如果你碰到了冲突，但发现搞不定，不要惊慌。只要执行下面这条命令，就可以回到你执行git pull --rebase命令前的样子：

git rebase --abort


### 小红成功发布功能


小红完成和中央仓库的同步后，就能成功发布她的修改了：

git push origin master

下一站

如你所见，仅使用几个Git命令我们就可以模拟出传统Subversion开发环境。对于要从SVN迁移过来的团队来说这太好了，但没有发挥出Git分布式本质的优势。

如果你的团队适应了集中式工作流，但想要更流畅的协作效果，绝对值得探索一下功能分支工作流的收益。通过为一个功能分配一个专门的分支，能够做到一个新增功能集成到正式项目之前对新功能进行深入讨论。

