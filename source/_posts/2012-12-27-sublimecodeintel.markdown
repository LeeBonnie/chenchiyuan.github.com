---
layout: post
title: "SublimeCodeIntel"
date: 2012-12-27 00:28
comments: true
categories: [sublime] 
---


## 这货很强大
[SublimeCodeIntel](https://github.com/Kronuz/SublimeCodeIntel)是一个非常强大的代码提示插件, 用上你就会喜欢上他。
<!--more-->


## 安装
使用Package Control安装，Command+Shift+p唤醒交互, 输入SublimeCodeIntel安装即可

## 使用
非常类似于Pycharm, Eclipse, Xcode的提示功能。这没什么好说的，自己体会下吧。


## 注意事项(Virtualenv的集成)
使用这货唯一要注意的就是Python环境下的Virtualenv的集成。从[说明](https://github.com/Kronuz/SublimeCodeIntel)可以看出，SublimeCodeIntel是通过读取~/.codeintel/config 或者 project_root/.codeintel/config来配置解释器的路径。所以，我们要把Virtualenv的路径加入其中。



在你的项目根路径下创建.codeintel/config文件，开始配置。

我的配置路径: (请配置自己的virtualenvs路径和app)
```
{
    # 注意Python的大小写
    "Python": { 
        # Python解释器路径
        "python": "$VIRTUALENVS/APP/bin/python", 

        # 第三方路径
        "pythonExtraPaths": [
            "$VIRTUALENVS/APP/lib/python2.7/site-packages", 
            "$VIRTUALENVS/APP/src"
        ], 
    }
}
```