---
layout: post
title: "用Sublime搭建你自己的Django IDE"
date: 2012-12-22 17:27
comments: true
categories: [sublime, ]
---

## 为什么使用Sublime?

开发Django已经有好几年了，想了想，中途使用的IDE还真是很多。使用比较长时间的莫过于Vim和Pycharm，为什么突然想使用Sublime来作为今后的主流IDE呢？不妨来对比下一下IDE的区别。
<!--more-->



#### Pycharm


优点：说实话，Pycharm是我目前为止觉得最适合Django的IDE，最突出的功能在于他的索引查找和代码提示。



缺点：吃内存，这个就不多说了，用的人都懂。


#### Vim 

首先，更正下大家的误区。Vim不只是一个文本编辑器，他强大的功能注定他能成为一款十分优秀的IDE。详细文章请参见 [谁说VIM不是IDE](http://www.cnblogs.com/chijianqiang/archive/2012/10/30/vim-1.html)


优点：非常舒服的快捷键设置，熟悉之后，能极大的提高工作效率。Bundle的管理也注定他能扩展很多非常优秀的功能。而且，在服务器上你也只能选择那么几个编辑器，Vim绝对是其中的佼佼者。


缺点：我不是骨灰级的Coder，所以骨子里不是很喜欢Vim和Emacs。如果哪天兴致来了想写下相应的扩展，Oh 天啦，还要学一堆东西，还是算了吧。。

#### 最后说说Sublime


Sublime我看中的是他的发展性，github上现在关于Sublime的扩展越来越多，没有多久，Pycharm和Vim的优点他都能集成进来。而且，他的扩展使用python开发，我技痒的时候，也可以玩玩。能定制自己的IDE，绝对很幸福。



## 定制自己的Sublime for Django (持续更新中。。) 


### 我理想中的功能。


    1.  方便的包管理。 
    2.  Vim式的跳转。
    3.  代码引用跳转。
    4.  zencoding的体验。


####  1. 包管理工具([Package Control](http://wbond.net/sublime_packages/package_control)(必装)
这个东西类似于什么呢？ apt-get, brew, pip, npm, yum .....


在sublime中通过组合键ctrl+~ 唤醒Python，然后输入

>import urllib2,os; pf='Package Control.sublime-package'; ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler())); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'


从此包管理变得简单，command+p 键入install 唤醒Package Control, 你就可以对自己的安装包进行管理了。很方便吧。

####  2. Vim跳转(选装)
经过实验，我觉得Vintage([github](https://github.com/sublimehq/Vintage))是最合适的Sublime Vim Plugin。


安装方法如下：
>cd ~/Library/Application\ Support/Sublime\ Text\ 2/Packages/
git clone git@github.com:sublimehq/Vintage.git "Vintage Dev"


安装之后，你可以通过Esc唤醒Vim模式。是不是突然有种使用Vim的感觉啊~


####  3. Ctags跳转 [看详细版本吧](/blog/2012/12/26/sublimezhi-ctags/)(必装)
Sublime本身支持一些函数级和文件级的跳转。

**文件跳转** Command+p 唤醒交互，输入文件名即可。


**文件内函数跳转** Command+p 输入@。


但是这些还没解决我的最大需求，我需要的是函数级别的跳转。举个例子，很想从**from django.db import models**中查看下源代码models的实现。怎么办呢？千万不要傻乎乎
的去找pip site-packages下的django目录，然后逐级的找到models。要记住，你是程序员。


上网找了下，发现Ctags是理想的工具，而且他提供Sublime的支持。



直接在Package Control里安装即可。按照说明，Control+Shift+左键狂点函数名，还是没有跳转过去。。顿时泪流满面。这是为什么了。


仔细研究了下，Ctags的原理其实很简单，他会将索引存放在.tags文件里面，之所以找不到相应的位置，是没有索引住。默认情况下他不会去找virtualenvs下的配置。好吧。感觉这个可以开个另外的博文来详细讲讲----[解决方法](/blog/2012/12/26/sublimezhi-ctags/)


#### 4. SublimeCodeIntel(非常优秀的代码提示, 必装)
直接看我的[博客](/blog/2012/12/27/sublimecodeintel/)
