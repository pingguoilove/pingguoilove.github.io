---
layout: post
title: "CocoaPods简介"
author: 张小军
published: false
date: 2014-01-23 12:32
comments: true
categories: 
- 技术
- 翻译
- raywenderlich
- iOS7
- CocoaPods
---


注1： 本文由张小军译自<span style="color: green;">[introduction-to-cocoapods][1]</span>, 你也可以阅读<span style="color: green;">[俄文版][2]</span>  
_原文教程由**[raywenderlich][3]**教程团队成员**[Marcelo Fabri][4]**提供,**[Marcelo Fabri][4]**是一位在Movile工作的开发者，您可以浏览他的**[个人网站][5]**，或在**[Twitter][6]**,**[Google+][7]**上与他联系。_

在本教程中，你讲学会如何使用objective-c流行以的叫做_**[CocoaPods][8]**_的依赖管理工具.
<!-- more -->
##	**<span style="color: green;">目录</span>**    
**<span style="color: green;">1、安装CocoaPods</span>**  
**<span style="color: green;">2、安装CocoaPods</span>**  
**<span style="color: green;">3、安装CocoaPods</span>**  
**<span style="color: green;">4、安装CocoaPods</span>**  

##	**<span style="color: green;">正文</span>**  
先稍等一下!到底什么是**`依赖管理工具`**呢？我们又为什么需要用以来管理工具呢？

作为一个iOS开发者，你一定使用了许多别人写好的库中的一些代码。你可能不记得了，但是如果你在项目中不得不从头实现这些功能，你的生活将变得多么的糟糕。

平时（由于手动编译静态库太太太蛋疼了），你仅仅是将第三方库的代码添加到你的工程中。但是这样就有了许多不好的地方：
>*	代码可能还存在于你的代码仓库的其它地方，浪费空间
>*	有时候，很难获取到某个库的特定版本
>*	没有一个中心的地方来查询某个库是否可以获取得到
>*	找到一个库的新版本，然后更新到你的工程中去，是一件闲的蛋疼又痛苦的事情
>*	如果你下载库的源码，然后添加到工程中，并且对这个库做了一些定制化的修改，这样就更麻烦了，因为你在更新版本的时候需要手动去合并修改的地方到新的版本中
>

依赖管理工具可以帮助你解决上面提到的大部分问题。它将帮助你解决你在工程中用到的各个第三方库的依赖关系，下载依赖库源码，创建和维护正确的环境来保证你以最小的麻烦重新正确构建工程。

对于具有JavaEE开发背景的程序员来说，_**[Maven][9]**_是JavaEE最好的依赖管理工具。我也渴望着有一天我只需要指定我需要的库（以及库的具体版本）就自动可以将这些苦安装或者包含到每个工程中去了。

这一天终于到来了，当我发现**[CocoaPods][8]**的时候:]

在本教程中，你将会动手体验到使用**[CocoaPods][8]**。具体来说，你将会用来来创建一个应用程序连接到流行的电视网站trakt.tv，并且获取有关你（或者其他人）订阅该节目的信息，为你展现即将播出的节目信息。

正如你即将从本教程中看到的那样，使用CocoaPods将会使得这个工程构建更加容易。继续仔细阅读吧。


[1]:http://www.raywenderlich.com/12139/introduction-to-cocoapods
[2]:http://www.raywenderlich.com/ru/25225/%d0%92%d0%b2%d0%b5%d0%b4%d0%b5%d0%bd%d0%b8%d0%b5-%d0%b2-cocoapods
[3]:http://www.raywenderlich.com
[4]:http://www.raywenderlich.com/about#marcelofabri
[5]:http://www.marcelofabri.com/
[6]:https://twitter.com/marcelofabri_
[7]:https://plus.google.com/103477069214242099467?rel=author
[8]:http://www.cocoapods.org/
[9]:http://maven.apache.org/