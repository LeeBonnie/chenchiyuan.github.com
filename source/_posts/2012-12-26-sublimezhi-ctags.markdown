---
layout: post
title: "Sublime之Ctags"
date: 2012-12-26 21:32
comments: true
categories: [sublime]
---

## 前言
之前在博客里埋了一个坑，如何使用Ctags实现类似于Pycharm的点击跳转到定义处的功能。这个功能强大之处在于，你可以方便的寻找到源码，看它的注释，分析他的代码，这非常有助于你了解作者的逻辑，同时，也能展现给你优秀程序员的代码之美。
<!--more-->


## 怎么实现？
为了方便那些只求答案的同学，我将实现方式贴在下面。



假设你需要建立索引的源头有两个。
#### 1. 你的当前项目路径$PROJECT
#### 2. 你的Python源码以及第三方的根路径$SRC
```
1. ctags -R -f $PROJECT/.tags $PROJECT --languages=Python
2. ctags -R -f $PROJECT/.tags -a $SRC --languages=Python
```
以后每需要新增一个目录，都可以通过以下命令来实现
```
ctags -R -f $PROJECT/.tags - a $DIR --languages=Python
```
当然它可以从某个文件中读取相应的目录列表，我的需求目录只有两个，就没这样写配置。

## Ctags的介绍
ctags用官方的解释就是产生标记文件，帮助在文件中定位对象。其实就是你可以找到一个对象的定义处。

#### 实现原理
Ctags会将定义的对象索引化，记录他的出处。用Python伪代码来形容下
```
{
    'a': [path_a, path_b],
    'b': [path_c, ]
    ...
}
```
每当找到一个需要定义对象的时候，他都会在HashTable里面增加对象地址的记录。这样，以后扫元素的时候，它可以从列表中读出相应的位置，点击完成跳转。<font color="red">(个人理解，他的实现肯定更加严谨高效)</font>

#### 读读参数表
```
ctags --help
Usage: ctags [options] [file(s)]

  -a   Append the tags to an existing tag file.
  -B   Use backward searching patterns (?...?).
  -e   Output tag file for use with Emacs.
  -f <name>
       Write tags to specified file. Value of "-" writes tags to stdout
       ["tags"; or "TAGS" when -e supplied].
  -F   Use forward searching patterns (/.../) (default).
  -h <list>
       Specify list of file extensions to be treated as include files.
       [".h.H.hh.hpp.hxx.h++"].
  -I <list|@file>
       A list of tokens to be specially handled is read from either the
       command line or the specified file.
  -L <file>
       A list of source file names are read from the specified file.
       If specified as "-", then standard input is read.
  -n   Equivalent to --excmd=number.
  -N   Equivalent to --excmd=pattern.
  -o   Alternative for -f.
  -R   Equivalent to --recurse.
  -u   Equivalent to --sort=no.
  -V   Equivalent to --verbose.
  -x   Print a tabular cross reference file to standard output.
  --append=[yes|no]
       Should tags should be appended to existing tag file [no]?
  --etags-include=file
      Include reference to 'file' in Emacs-style tag file (requires -e).
  --exclude=pattern
      Exclude files and directories matching 'pattern'.
  --excmd=number|pattern|mix
       Uses the specified type of EX command to locate tags [pattern].
  --extra=[+|-]flags
      Include extra tag entries for selected information (flags: "fq").
  --fields=[+|-]flags
      Include selected extension fields (flags: "afmikKlnsStz") [fks].
  --file-scope=[yes|no]
       Should tags scoped only for a single file (e.g. "static" tags
       be included in the output [yes]?
  --filter=[yes|no]
       Behave as a filter, reading file names from standard input and
       writing tags to standard output [no].
  --filter-terminator=string
       Specify string to print to stdout following the tags for each file
       parsed when --filter is enabled.
  --format=level
       Force output of specified tag file format [2].
  --help
       Print this option summary.
  --if0=[yes|no]
       Should C code within #if 0 conditional branches be parsed [no]?
  --<LANG>-kinds=[+|-]kinds
       Enable/disable tag kinds for language <LANG>.
  --langdef=name
       Define a new language to be parsed with regular expressions.
  --langmap=map(s)
       Override default mapping of language to source file extension.
  --language-force=language
       Force all files to be interpreted using specified language.
  --languages=[+|-]list
       Restrict files scanned for tags to those mapped to langauges
       specified in the comma-separated 'list'. The list can contain any
       built-in or user-defined language [all].
  --license
       Print details of software license.
  --line-directives=[yes|no]
       Should #line directives be processed [no]?
  --links=[yes|no]
       Indicate whether symbolic links should be followed [yes].
  --list-kinds=[language|all]
       Output a list of all tag kinds for specified language or all.
  --list-languages
       Output list of supported languages.
  --list-maps=[language|all]
       Output list of language mappings.
  --options=file
       Specify file from which command line options should be read.
  --recurse=[yes|no]
       Recurse into directories supplied on command line [no].
  --regex-<LANG>=/line_pattern/name_pattern/[flags]
       Define regular expression for locating tags in specific language.
  --sort=[yes|no|foldcase]
       Should tags be sorted (optionally ignoring case) [yes]?.
  --tag-relative=[yes|no]
       Should paths be relative to location of tag file [no; yes when -e]?
  --totals=[yes|no]
       Print statistics about source and tag files [no].
  --verbose=[yes|no]
       Enable verbose messages describing actions on each source file.
  --version
       Print version identifier to standard output.
```
## 我知道你懒得看，我来讲讲几个重要的
#### 1. Ctags能识别哪些语言呢？
```
ctags --list-languages
一堆，自己看看。主流的基本都有了。
```

#### 2. 语言它会找相应的文件
```
ctags --list-maps
Python   *.py *.pyx *.pxd *.pxi *.scons # 以Python为例子
```

#### 3. Python它会识别什么语言元素？
```
ctags --list-kinds=Python
c  classes
f  functions
m  class members
v  variables
i  imports
```
可见，它会将类，函数，成员，变量和引入作为元素来生成索引。所以在Sublime里面你点击文件或者模块，他是不会跳转到相应的位置的(稍后研究下怎么解决)。

#### 4. 之前介绍命令的含义
```
-R 递归的寻找目录的子目录
-f 将索引写入指定文件，Sublime的插件Ctags读的是**当前视图的.tags文件**。
-a 追加索引
--languages 选择语言解释器
```
所以之前两句的解释是这样
```
ctags -R -f $PROJECT/.tags $PROJECT --languages=Python 
```
递归的将项目目录的文件使用Python解释器建立索引写入项目目录的.tags文件中

```
ctags -R -f $PROJECT/.tags -a $SRC --languages=Python
```
递归的将SRC目录的文件使用Python解释器建立索引追加到项目目录的.tags文件中
之后Sublime的插件Ctags会读取当前试图下的.tags文件，完成点击跳转的功能。


#### 5. 还不懂或者没解决问题？
自己Google吧，答案就在其中。




