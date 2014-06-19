---
layout: post
title: "hello github pages"
date: 2014-01-16 20:41:02 +0800
comments: true
categories: 
- octopress

---

前端时间一直在捣腾openshift([OpenShift](https://www.openshift.com)是来自Red Hat的平台及服务的云计算平台),但是openshift被GFW墙掉了，果断放弃。其实在10月份的时候就看到了[破船](http://beyondvincent.com/)的《[利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)》（不得不惊叹破船大哥是个高产的bloger，几个月不见博客数量蹭蹭蹭地涨，是广大技术宅男学习的榜样）,当时我的本本装的是OSX 10.7，不知道为什么一直装不上rbenv，当时项目紧张，就不得不放弃了。最近回到大本营，除了学习基本上没什么事情做，本本的系统也升级到10.9，就决定再试试在Github上搭建自己的博客。
   <!-- more -->
   搭建Github博客主要有以下几点好处：  

>1.	Github空间免费，稳定快速，无流量限制。 
>2.	离线写作，“像写程序一样写博客”。  
>3.	编写博客使用Markdown，简单易学。  
>4.  提交博客使用命令行可以进一步学习git知识。
>5.  多人协作撰写。 
 
###摘要
*	安装Octopress
*	配置Octopress
*	配置Github Pages
*	预览博客开发环境效果
*	部署博客到Github
*	在开发环境创建新博文并部署到Github
*	为Github Pages添加一个自己的域名

###环境
*ruby 2.0.0p247*, *MAC OSX 10.9*, *Mou*

###安装步骤
1.	安装Ruby，安装Octopress，配置Octopress请参见破船的《[利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)》。

2.	配置Github Pages。
	
	*	要创建组织主页（Org Pages）或个人主页（User），首先你需要创建一个名称为username.github.io 或者orgname.github.io的代码仓库。username或者orgname必须为github的用户名，否则无法生产页面。

	*	当名称为username.github.io 或者orgname.github.io的代码仓库创建好后，使用Automic Page Generator生成页面。请参考[此文](http://tech.marsw.tw/blog/2012/11/24/setup-octopress-on-windows-from-zero-to-1)。
	
	*	代码仓库创建好后，需要生成和配置SSH key，否则，在使用git相关命令与远程代码仓库交互的时候有可能会报Error:Permission xxx之类的错误（很不幸，我遇到了）。生成和配置SSH Key请参考[GitHub Help](https://help.github.com/articles/generating-ssh-keys) 注意：请确保执行了ssh-keygen和ssh-add。

3.	预览博客效果可以运行。

	rake preview  
然后在浏览器中输入localhost:4000查看效果。

4.	部署博客到Github也请参考破船的《[利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)》。
	
	*	在我运行 rake deploy的时候，提交失败，当前分支不是的版本不是最新的，解决方法请参考[此文](http://yeesterbunny.github.io/blog/2013/08/21/what-i-learned-from-hosting-octopress-on-github/)。
	*	在执行更新当前分支的命令的时候(git pull origin master)，会提示index.html有冲突，请可以使用(git mergetool)命令打开一个GUI工具来解决冲突。参考SO上面的[这个问题](http://stackoverflow.com/questions/161813/how-do-i-fix-merge-conflicts-in-git)。
	*	当rake deploy成功后，使用username.github.io 或者orgname.github.io访问到自己的Github Pages了。

5.	其余部分，均请参考破船的文章《[利用Octopress搭建一个Github博客](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/)》。
6.	安装主题，[这里](http://opthemes.com/)搜集了很多漂亮的octopress主题，安装方法也有。  
{% codeblock 博客撰写，发布的流程基本如下了 %}
rake new_post["text name"]
编辑生成的markdown文件
rake preview预览效果
修改博客
rake generate
git add .
git commit -am 'your commments'
git push origin source
rake deploy(若提示persmission问题，请用sudo rake deploy命令)
{% endcodeblock %}

###总结
本文用Mou来写的，Markdown语法很不熟练，需要进一步学习Markdown语法。另外，今天学习到了很多git的知识。

###接下来
接下来，首先了解一下octopress，争取把博客做得漂亮好用点，自己写作也有个好心情；然后就是就好好总结一下自己所看，所学，所写的东西，都记录到博客里，作为自己的知识库。希望能提高自己对技术的理解深度，以及写作能力。

###References
1.	[http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/](http://beyondvincent.com/blog/2013/08/03/108-creating-a-github-blog-using-octopress/){:target="_blank"}
2.	[http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/](http://blog.devtang.com/blog/2012/02/10/setup-blog-based-on-github/){:target="_blank"}
3.	[http://www.tomordonez.com/blog/2012/06/04/creating-a-github-blog-using-octopress/](http://www.tomordonez.com/blog/2012/06/04/creating-a-github-blog-using-octopress/){:target="_blank"}
4.	[http://octopress.org/docs/](http://octopress.org/docs/){:target="_blank"}
5.	[http://tech.marsw.tw/blog/2012/11/24/setup-octopress-on-windows-from-zero-to-1](http://tech.marsw.tw/blog/2012/11/24/setup-octopress-on-windows-from-zero-to-1){:target="_blank"}
6.	[http://opthemes.com/](http://opthemes.com/){:target="_blank"}
7.	[http://tech.paulcz.net/2012/12/creating-a-github-pages-blog-with-octopress.html](http://tech.paulcz.net/2012/12/creating-a-github-pages-blog-with-octopress.html "如何从一个已有的Octopress博客开始搭建"){:target="_blank"}




 