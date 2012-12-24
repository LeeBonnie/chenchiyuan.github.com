---
layout: post
title: "数据导出从此无忧:tablib"
date: 2012-12-24 23:39
comments: true
categories: [projects, ] 
tags: [tablib,]
---

## 前言 
最近很喜欢在Github上搜搜项目，特别是那些短小，优美，强大的项目，不仅用起来很贴心，读起源码来也是说不出的舒服。今天时间不早了，写一篇小短文算是开个头。
<!--more-->



## 推荐一个人先
如果你玩Python, 如果同时你也玩Github。建议你关注下这个人[kennethreitz](https://github.com/kennethreitz)。介绍他只需要用一句话He wrote requests。
如果你不知道[requests](https://github.com/kennethreitz/requests)是什么，好吧，点进去看看，基本你用一次，以后就不会用其他的网络库了。


For Humans是kennethreitz的追求。短短两个单词，却让我想到了好多东西。程序员就应该像他那样，做出这些看起来美，用起来美，真正能服务于人的东西。扯多了，先说正题。


## tablib是啥子？
>Tablib is a format-agnostic tabular dataset library, written in Python.

Tablib是一个数据类，如果你使用他的格式定义数据，你可以直接将数据导出成XLSX, XLS, ODS, JSON, YAML, CSV, TSV, and HTML(**注意，可没说有XML哈**)等格式。


用代码说话更清楚：
```
import tablib
headers = ('first_name', 'last_name')

data = [
    ('John', 'Adams'),
    ('George', 'Washington')
]

data = tablib.Dataset(*data, headers=headers)
>>> data.append(('Henry', 'Ford'))
>>> data.append_col((90, 67, 83), header='age')
print data[:2]
[('John', 'Adams', 90), ('George', 'Washington', 67)]
>>> print data['first_name']
['John', 'George', 'Henry']

```
**Json**
```
>>> print data.json
[
  {
    "last_name": "Adams",
    "age": 90,
    "first_name": "John"
  },
  {
    "last_name": "Ford",
    "age": 83,
    "first_name": "Henry"
  }
]
```
**CSV**
```
>>> print data.csv
first_name,last_name,age
John,Adams,90
Henry,Ford,83
```
**Excel**(话说我也写过，和这个tablib一对比，果断删掉重写= =)
```
>>> open('people.xls', 'wb').write(data.xls)
```

## 它能做什么？
我见到这货第一反应是：如果我写了一层Django Model和tablib的Adapter之后，以后数据的导出就再也不费功夫了，Model的数据都可以自行的输出成理想的格式，太美妙了。瞬间有了实现的冲动。冷静的去github上瞅了一眼，我想到的大家几年前就想到了= =详情请看[django-tablib传送门](https://github.com/joshourisman/django-tablib)


用处不限于此，如果你经常需要将输入导入导出，建议你好好想想这货可以怎么帮你。这提高的效率可不是一点点。


## 安装
```
$ pip install tablib
```

或者
```
$ easy_install tablib
```

