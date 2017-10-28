---
layout:     post
title:      "Git工作流指南"
subtitle:   "功能分支工作流"
date:        2017-10-27 10:00:00
author:     ""
header-img: "img/post-bg-2015.jpg"
tags:
   - git, 工作流
---


一旦你玩转了集中式工作流，在开发过程中可以很简单地加上功能分支，用来鼓励开发者之间协作和简化交流。

功能分支工作流背后的核心思路是所有的功能开发应该在一个专门的分支，而不是在master分支上。这个隔离可以方便多个开发者在各自的功能上开发而不会弄乱主干代码。另外，也保证了master分支的代码一定不会是有问题的，极大有利于集成环境。

功能开发隔离也让pull requests工作流成功可能，pull requests工作流能为每个分支发起一个讨论，在分支合入正式项目之前，给其它开发者有表示赞同的机会。另外，如果你在功能开发中有问题卡住了，可以开一个pull requests来向同学们征求建议。这些做法的重点就是，pull requests让团队成员之间互相评论工作变成非常方便！


## 工作方式

功能分支工作流仍然用中央仓库，并且master分支还是代表了正式项目的历史。但不是直接提交本地历史到各自的本地master分支，开发者每次在开始新功能前先创建一个新分支。功能分支应该有个有描述性的名字，比如animated-menu-items或issue-#1061，这样可以让分支有个清楚且高聚焦的用途。

在master分支和功能分支之间，Git是没有技术上的区别，所以开发者可以用和集中式工作流中完全一样的方式编辑、暂存和提交修改到功能分支上。

另外，功能分支也可以（且应该）push到中央仓库中。这样不修改正式代码就可以和其它开发者分享提交的功能。由于master仅有的一个『特殊』分支，在中央仓库上存多个功能分支不会有任何问题。当然，这样做也可以很方便地备份各自的本地提交。


## Pull Requests


功能分支除了可以隔离功能的开发，也使得通过Pull Requests讨论变更成为可能。一旦某个开发完成一个功能，不是立即合并到master，而是push到中央仓库的功能分支上并发起一个Pull Request请求去合并修改到master。在修改成为主干代码前，这让其它的开发者有机会先去Review变更。

Code Review是Pull Requests的一个重要的收益，但Pull Requests目的是讨论代码一个通用方式。你可以把Pull Requests作为专门给某个分支的讨论。这意味着可以在更早的开发过程中就可以进行Code Review。比如，一个开发者开发功能需要帮助时，要做的就是发起一个Pull Request，相关的人就会自动收到通知，在相关的提交旁边能看到需要帮助解决的问题。

一旦Pull Request被接受了，发布功能要做的就和集中式工作流就很像了。首先，确定本地的master分支和上游的master分支是同步的。然后合并功能分支到本地master分支并push已经更新的本地master分支到中央仓库。

仓库管理的产品解决方案像Bitbucket或Stash，可以良好地支持Pull Requests。可以看看Stash的Pull Requests文档。


## 示例


下面的示例演示了如何把Pull Requests作为Code Review的方式，但注意Pull Requests可以用于很多其它的目的。

小红开始开发一个新功能


在开始开发功能前，小红需要一个独立的分支。使用下面的命令新建一个分支：

git checkout -b marys-feature master

这个命令检出一个基于master名为marys-feature的分支，Git的-b选项表示如果分支还不存在则新建分支。这个新分支上，小红按老套路编辑、暂存和提交修改，按需要提交以实现功能：

git status
git add
git commit

## 小红要去吃个午饭


早上小红为新功能添加一些提交。去吃午饭前，push功能分支到中央仓库是很好的做法，这样可以方便地备份，如果和其它开发协作，也让他们可以看到小红的提交。

git push -u origin marys-feature

这条命令push marys-feature分支到中央仓库（origin），-u选项设置本地分支去跟踪远程对应的分支。设置好跟踪的分支后，小红就可以使用git push命令省去指定推送分支的参数。


## 小红完成功能开发


小红吃完午饭回来，完成整个功能的开发。在合并到master之前，她发起一个Pull Request让团队的其它人知道功能已经完成。但首先，她要确认中央仓库中已经有她最近的提交：

git push

然后，在她的Git GUI客户端中发起Pull Request，请求合并marys-feature到master，团队成员会自动收到通知。Pull Request很酷的是可以在相关的提交旁边显示评注，所以你可以很对某个变更集提问。


## 小黑收到Pull Request

小黑收到了Pull Request后会查看marys-feature的修改。决定在合并到正式项目前是否要做些修改，且通过Pull Request和小红来回地讨论。


##  小红再做修改


要再做修改，小红用和功能第一个迭代完全一样的过程。编辑、暂存、提交并push更新到中央仓库。小红这些活动都会显示在Pull Request上，小黑可以断续做评注。

如果小黑有需要，也可以把marys-feature分支拉到本地，自己来修改，他加的提交也会一样显示在Pull Request上。

## 小红发布她的功能


一旦小黑可以的接受Pull Request，就可以合并功能到稳定项目代码中（可以由小黑或是小红来做这个操作）：

git checkout master
git pull
git pull origin marys-feature
git push

无论谁来做合并，首先要检出master分支并确认是它是最新的。然后执行git pull origin marys-feature合并marys-feature分支到和已经和远程一致的本地master分支。你可以使用简单git merge marys-feature命令，但前面的命令可以保证总是最新的新功能分支。最后更新的master分支要重新push回到origin。

这个过程常常会生成一个合并提交。有些开发者喜欢有合并提交，因为它像一个新功能和原来代码基线的连通符。但如果你偏爱线性的提交历史，可以在执行合并时rebase新功能到master分支的顶部，这样生成一个快进（fast-forward）的合并。

一些GUI客户端可以只要点一下『接受』按钮执行好上面的命令来自动化Pull Request接受过程。如果你的不能这样，至少在功能合并到master分支后能自动关闭Pull Request。


##  与此同时，小明在做和小红一样的事

当小红和小黑在marys-feature上工作并讨论她的Pull Request的时候，小明在自己的功能分支上做完全一样的事。

通过隔离功能到独立的分支上，每个人都可以自主的工作，当然必要的时候在开发者之间分享变更还是比较繁琐的。


## 下一站

到了这里，但愿你发现了功能分支可以很直接地在集中式工作流的仅有的master分支上完成多功能的开发。另外，功能分支还使用了Pull Request，使得可以在你的版本控制GUI客户端中讨论某个提交。

功能分支工作流是开发项目异常灵活的方式。问题是，有时候太灵活了。对于大型团队，常常需要给不同分支分配一个更具体的角色。Gitflow工作流是管理功能开发、发布准备和维护的常用模式。

