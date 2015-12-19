---
layout:     post
title:      "Android Studio你不知道的快捷键(三)"
subtitle:   " \"android studio\""
date:       2015-12-19 21:00:00
author:     ""
header-img: "img/post-bg-2015.jpg"
tags:
##     - android


 

# Android Studio你不知道的快捷键(三)

## Select In..
说实话，想不出一个比较好的翻译 :P 干脆使用英文吧。

![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-1.gif "Optional title")

有没有这样的场景：你在Android Studio打开了一个图片文件（或者别的文件），想在资源浏览器里面查看这图片；在Eclipse里面我想大部分的人是Alt + Enter进入文件属性复制地址，然后在系统资源管理器里面打开；或者装一个EasyExplore插件。在Android Studio里面，这是内建支持的！而且还不止如此！比如你想看看某个文件在包的哪个目录，通常是不是点击Project View上面的那个小圆坐标；用这个快捷键鼠标就能搞定。

快捷键: Alt + F1

弹出的菜单有一系列的选项；按对应的数字就可以选择；其他的菜单有什么功能可以自己尝试一下。

## 拓展选择

![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-2.gif "Optional title")

这个功能应该很多人都知道；但还是说明一下，因为跟下面两个功能跟这个结合起来才有威力。这个功能太强大了，自己去按几遍就能想到很多使用场景了；我相信有了这个功能，你使用鼠标的机会会少很多。

Mac: Alt + up/down
Win/Linux: ctrl + w / ctrl + shift + w

## Surround With..

![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-3.gif  "Optional title")

如果你想把一段代码使用if语句包起来；又或者使用try包围一段可能有运行时异常的代码，你会怎么干？

首先用光标定位到代码块开头，写上 try {然后，光标代码块末尾加上} catch (XXXRuntimeException e) {// todo}？可以试试这个快捷键。

Mac: cmd + alt + t
Win/Linux: ctrl + alt + t
可以使用上面的拓展选择选择你需要的代码块，然后使用这个功能Surround With；如果你什么都不选择的话，那么默认选择的是光标所在行。

## Unwrap/Remove


![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-4.gif  "Optional title")

这个功能跟上面提到的是一对，有了Surround With自然就有Unsurround With;使用情况没有上面那个那么多，但是好歹一对，一起介绍吧。

## 高亮某东西

![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-5.gif "Optional title")

有时候看代码的时候，看到某个变量想知道哪里使用了它；你还在用肉眼查找吗？或者你杀鸡用牛刀Find Usgae？其实你的需求就是把这个变量全部给我打个标签，我想直观的知道它在哪。

Mac: cmd + shift + F7
Win/Linux: ctrl + shift + F7
这个键功能远不止这个！

如果你高亮return或者throw，那么会把这个方法所有的返回点高亮出来！
高亮某个类的extends或者implements会把这个类Override的方法高亮出来
高亮import会把使用的地方显示出来
如果不想要高亮了，按下Esc就行。

## 显示方法调用树

![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-6.gif "Optional title")

在看源码的时候，你还是使用alt + F7和ctrl + B在各个类之间来回穿梭吗？其实好多时候你就是想知道这个调用结构是怎么样的而已；谁是怎么一步一步滴调用谁的；这个快捷键会给你一个调用树。有了这个大菊观，继续探讨就很容易了。

快捷键: ctrl + alt + h

## 万能快捷键

![Alt text](http://7xp3xc.com1.z0.glb.clouddn.com/201512-17-7.gif "Optional title")

记得之前提到过一个万能重构键, 有关重构的一切操作都可通过它完成。那么Android Studio这么快捷键，这么多功能，臣妾怎么可能都记住！要是有万能钥匙就好了！That’s it!

使用这个快捷键，你想到什么功能，打开它搜索就可以了；打个比方，我想看看Java的for each循环和普通的for循环底层是不是同一个实现;那么我就需要看虚拟机字节码了；我记得有这个功能但是不知道快捷键是啥；OK，<font color="gray">Cmd + shift + A</font>，输入bytecode:

![Alt text](/http://7sbqce.com1.z0.glb.clouddn.com/test/1450323104893.png "Optional title")

PS:(我用的Intellij IDEA，Android Studio没有集成bytecode功能，可能搜索不到）

好了，其实所有的快捷键的功能都可以用这个搜索到～～实在记不起来也就用万能键吧！

