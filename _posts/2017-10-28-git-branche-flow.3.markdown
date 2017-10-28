---
layout:     post
title:      "Git工作流指南"
subtitle:   "Pull Request工作流"
date:        2017-10-27 10:20:00
author:     ""
header-img: "img/post-bg-2015.jpg"
tags:
   - git, 工作流
---

Pull Requests是Bitbucket上方便开发者之间协作的功能。提供了一个用户友好的Web界面，在集成提交的变更到正式项目前可以对变更进行讨论。

开发者向团队成员通知功能开发已经完成，Pull Requests是最简单的用法。开发者完成功能开发后，通过Bitbucket账号发起一个Pull Request。这样让涉及这个功能的所有人知道，要去做Code Review和合并到master分支。

但是，Pull Request远不止一个简单的通知，而是为讨论提交的功能的一个专门论坛。如果变更有任何问题，团队成员反馈在Pull Request中，甚至push新的提交微调功能。所有的这些活动都直接跟踪在Pull Request中。

![1](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-overview.png)

相比其它的协作模型，这种分享提交的形式有助于打造一个更流畅的工作流。SVN和Git都能通过一个简单的脚本收到通知邮件；但是，讨论变更时，开发者通常只能去回复邮件。这样做会变得杂乱，尤其还要涉及后面的几个提交时。Pull Requests把所有相关功能整合到一个和Bitbucket仓库界面集成的用户友好Web界面中。



## 解析Pull Request

当要发起一个Pull Request，你所要做的就是请求（Request）另一个开发者（比如项目的维护者），来pull你仓库中一个分支到他的仓库中。这意味着你要提供4个信息（源仓库、源分支、目的仓库、目的分支），以发起Pull Request。

![s](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-anatomy.png)

这些值多数Bitbucket都会设置上合适的缺省值。但取决你用的协作工作流，你的团队可能会要指定不同的值。上图显示了一个Pull Request请求合并一个功能分支到正式的master分支上，但可以有多种不同的Pull Request用法。

## 工作方式


Pull Request可以和功能分支工作流、Gitflow工作流或Forking工作流一起使用。但Pull Request要求要么分支不同，要么仓库不同，所以不能用于集中式工作流。在不同的工作流中使用Pull Request会有一些不同，但基本的过程是这样的：

- 开发者在本地仓库中新建一个专门的分支开发功能。
- 开发者push分支修改到公开的Bitbucket仓库中。
- 开发者通过Bitbucket发起一个Pull Request。
- 团队的其它成员review code，讨论并修改。
- 项目维护者合并功能到官方仓库中并关闭Pull Request。
本文后面内容说明，Pull Request在不同协作工作流中如何应用。

## 在功能分支工作流中使用Pull Request


功能分支工作流用一个共享的Bitbucket仓库来管理协作，开发者在专门的分支上开发功能。但不是立即合并到master分支上，而是在合并到主代码库之前开发者应该开一个Pull Request发起功能的讨论。

功能分支工作流只有一个公开的仓库，所以Pull Request的目的仓库和源仓库总是同一个。通常开发者会指定他的功能分支作为源分支，master分支作为目的分支。

收到Pull Request后，项目维护者要决定如何做。如果功能没问题，就简单地合并到master分支，关闭Pull Request。但如果提交的变更有问题，他可以在Pull Request中反馈。之后新加的提交也会评论之后接着显示出来。

在功能还没有完全开发完的时候，也可能发起一个Pull Request。比如开发者在实现某个需求时碰到了麻烦，他可以发一个包含正在进行中工作的Pull Request。其它的开发者可以在Pull Request提供建议，或者甚至直接添加提交来解决问题。

##  在Gitflow工作流中使用Pull Request


Gitflow工作流和功能分支工作流类似，但围绕项目发布定义一个严格的分支模型。在Gitflow工作流中使用Pull Request让开发者在发布分支或是维护分支上工作时，可以有个方便的地方对关于发布分支或是维护分支的问题进行交流。

![q](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/gitflow-workflow-pull-request.png)

Gitflow工作流中Pull Request的使用过程和上一节中完全一致：当一个功能、发布或是热修复分支需要Review时，开发者简单发起一个Pull Request，团队的其它成员会通过Bitbucket收到通知。

新功能一般合并到develop分支，而发布和热修复则要同时合并到develop分支和master分支上。Pull Request可能用做所有合并的正式管理。


##  在Forking工作流中使用Pull Request

在Forking工作流中，开发者push完成的功能到他自己的仓库中，而不是共享仓库。然后，他发起一个Pull Request，让项目维护者知道他的功能已经可以Review了。

在这个工作流，Pull Request的通知功能非常有用，因为项目维护者不可能知道其它开发者在他们自己的仓库添加了提交。

![a](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-forking-workflow-1.png)


由于各个开发有自己的公开仓库，Pull Request的源仓库和目标仓库不是同一个。源仓库是开发者的公开仓库，源分支是包含了修改的分支。如果开发者要合并修改到正式代码库中，那么目标仓库是正式仓库，目标分支是master分支。

Pull Request也可以用于正式项目之外的其它开发者之间的协作。比如，如果一个开发者和一个团队成员一起开发一个功能，他们可以发起一个Pull Request，用团队成员的Bitbucket仓库作为目标，而不是正式项目的仓库。然后使用相同的功能分支作为源和目标分支。

![s](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-forking-workflow-2.png)


2个开发者之间可以在Pull Request中讨论和开发功能。完成开发后，他们可以发起另一个Pull Request，请求合并功能到正式的master分支。在Forking工作流中，这样的灵活性让Pull Request成为一个强有力的协作工具。


##  示例


下面的示例演示了Pull Request如何在在Forking工作流中使用。也同样适用于小团队的开发协作和第三方开发者向开源项目的贡献。

在示例中，小红是个开发，小明是项目维护者。他们各自有一个公开的Bitbucket仓库，而小明的仓库包含了正式工程。

##  小红fork正式项目

![a](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-1.png)

小红先要fork小明的Bitbucket仓库，开始项目的开发。她登陆Bitbucket，浏览到小明的仓库页面，
点Fork按钮

![a](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-2.png)

然后为fork出来的仓库填写名字和描述，这样小红就有了服务端的项目拷贝了。

##  小红克隆她的Bitbucket仓库

![a](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-3.png)


下一步，小红克隆自己刚才fork出来的Bitbucket仓库，以在本机上准备出工作拷贝。命令如下：

git clone https://user@bitbucket.org/user/repo.git

请记住，git clone会自动创建origin远程别名，是指向小红fork出来的仓库。


## 小红开发新功能

![a](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-4.png)



在开始改代码前，小红要为新功能先新建一个新分支。她会用这个分支作为Pull Request的源分支。


git checkout -b some-feature



## 编辑代码


git commit -a -m "Add first draft of some feature"

在新功能分支上，小红按需要添加提交。甚至如果小红觉得功能分支上的提交历史太乱了，她可以用交互式rebase来删除或压制提交。对于大型项目，整理功能分支的历史可以让项目维护者更容易看出在Pull Request中做了什么内容。

## 小红push功能到她的Bitbucket仓库中

![s](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-5.png)


小红完成了功能后，push功能到她自己的Bitbucket仓库中（不是正式仓库），用下面简单的命令：

git push origin some-branch

这时她的变更可以让项目维护者看到了（或者任何想要看的协作者）。



## 小红发起Pull Request

![A](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/example-6.png)


Bitbucket上有了她的功能分支后，小红可以用她的Bitbucket账号浏览到她的fork出来的仓库页面，点右上角的【Pull Request】按钮，发起一个Pull Request。弹出的表单自动设置小红的仓库为源仓库，询问小红以指定源分支、目标仓库和目标分支。

小红想要合并功能到正式仓库，所以源分支是她的功能分支，目标仓库是小明的公开仓库，而目标分支是master分支。另外，小红需要提供Pull Request的标题和描述信息。如果需要小明以外的人审核批准代码，她可以把这些人填在【Reviewers】文本框中。

![A](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-7.png)


创建好了Pull Request，通知会通过Bitbucket系统消息或邮件（可选）发给小明。



## 小明review Pull Request


![A](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-8.png)


在小明的Bitbucket仓库页面的【Pull Request】Tab可以看到所有人发起的Pull Request。点击小红的Pull Request会显示出Pull Request的描述、功能的提交历史和每个变更的差异（diff）。

如果小明想要合并到项目中，只要点一下【Merge】按钮，就可以同意Pull Request并合并到master分支。

但如果像这个示例中一样小明发现了在小红的代码中的一个小Bug，要小红在合并前修复。小明可以在整个Pull Request上加上评注，或是选择历史中的某个提交加上评注。

![A](https://raw.githubusercontent.com/quickhack/translations/master/git-workflows-and-tutorials/images/pull-request-9.png)

## 小红补加提交

如果小红对反馈有任何疑问，可以在Pull Request中响应，把Pull Request当作是她功能讨论的论坛。

小红在她的功能分支新加提交以解决代码问题，并push到她的Bitbucket仓库中，就像前一轮中的做法一样。这些提交会进入的Pull Request，小明在原来的评注旁边可以再次review变更。

## 小明接受Pull Request


最终，小明接受变更，合并功能分支到master分支，并关闭Pull Request。至此，功能集成到项目中，其它的项目开发者可以用标准的git pull命令pull这些变更到自己的本地仓库中。

## 下一站

到了这里，你应该有了所有需要的工具来集成Pull Request到你自己的工作流。请记住，Pull Request并不是为了替代任何基于Git的协作工作流，而是它们的一个便利的补充，让团队成员间的协作更轻松方便。

