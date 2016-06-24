---
layout:     post
title:      "android 面试ListView"
subtitle:   "android面试"
date:        2016-06-22 09:00:00
author:     ""
header-img: "img/post-bg-android.png"
tags:
   - android interview
---




# 面试题：如何优化ListView的性能？

在回答这个问题前，我认为很有必要和大家讲几点和getView相关的问题。我们设置或者优化ListView的性能很多时候都是在getView中完成的，反过来说就是很多性能问题都是由于没有正确使用getView造成的。

    public View getView(int position, View convertView, ViewGroup parent)

所以我们不妨先思考一下如下的几个问题：

    在一次显示ListView的界面时，getView会被执行几次？
    每次getView执行时间应该控制在多少毫秒之内？
    getView中设置listener要注意什么？

首先我们要知道ListView的ItemView有一个复用机制，简单看如下图所示，ListView中有一个RecycleBin类复负回收不可见且可能被再次使用的ItemView，由ScrapView存储。


![][http://upload-images.jianshu.io/upload_images/1685558-73c9e629b9b0e273.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240］



所以我在们设置Listener进就要注意，使用convertView时需要重新设置一个Listener，保括一些数据也需要重设置，不然可能会显示之前那个ItemView在回收前的状态。

在绘制ListView前往往要计算它的高度，所以一个ListView界面上可以看到6个ItemView，但是getView的执行次数却有可能是12次，多出的次数用来计算高度（这个可以通过设置ListView的height为0来避免）。所以要避免在getView中进行逻辑运算，两次计算同一逻辑完全是浪费。


每个getView的执行时间更是少得可怜，很多人可能对这个时间没有概念，我可以简单的给大算一下


    1秒之内屏幕可以完成30帧的绘制，人才能看到它比较流畅（苹果是接近60帧，高于60之后人眼也无法分辨）。
    每帧可使用的时间：1000ms/30 = 33.33 ms
    每个ListView一般要显示6个ListItem，加上1个重用convertView：33.33ms/7 = 4.76ms

即是说，每个getView要在4.76ms内完成工作才会较流畅，但是事实上，每个getView间的调用也会有一定的间隔（有可能是由于handler在处理别的消息），UI的handler处理不好的话，这个间隔也可难会很大（0ms-200ms）。结论就是，留给getView使用的时间应该在4ms之内，如果不能控制在这之内的话，ListView的滑动就会有卡顿的现象。

了解了这几个问题，现在我们回来这次主要考查的面试题上，如何进行ListView的性能优化，让它滑动更加流畅。大家一般常用如下方法：


    1.重用ConvertView;
    2.使用View Holder模式；
    3.使用异步线程加载图片（一般都是直接使用图片库加载，如Glide, Picasso）；

我认为这些是面试者必备的知识点，如果连这些都说不清楚的话，也没有必要再深入问了。针对面试者的回答，可以适当选一两点追问一下，看是否真正明白。如：ViewHolder为什么能够起到优化性能的作用？


除此之前还有一些优化建议：

    1.在adapter的getView方法中尽可能的减少逻辑判断，特别是耗时的判断；
    2.避免GC（可以从LOGCAT查看有无GC的LOG）；
    3.在快速滑动时不要加载图片；
    4.将ListView的scrollingCache和animateCache这两个属性设置为false（默认是true）   
    5.尽可能减少List Item的Layout层次（如可以使用RelativeLayout替换LinearLayout，或使用自定的View代替组合嵌套使用的Layout）；


关于第4点，发现在一些型号的手机（如华为的P7）上特别管用，当其也优化都做完之后，有无这两项设置滑动的卡顿情况有明显不同。


关于第4点，发现在一些型号的手机（如华为的P7）上特别管用，当其也优化都做完之后，有无这两项设置滑动的卡顿情况有明显不同。

    <Listview
     android:scrollingCache="false" 
     android:animationCache="false"

##小结


关于ListView有很多方面可以考察面试者，因为它实在是用的太频繁了，双方都能对某个问题点进行展开。如果一个面试者都没有做过ListView优化，那么如果不是他写的代码太少就是他使用ListView加载的数据太简单（可能只有几十项），其本上没有其他选项。所以这一题是很能看出面试者的项目经验和实际的开发水平，属于面试必考题之一。

