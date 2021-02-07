---
title: '使用git进行word版本管理'
date: '2021-02-04 19:36:00'
---

## 系统环境

win10 + git 2.29 + pandoc 2.？（RStudio自带）

## 安装pandoc

安装RStudio时自带pandoc，也可另行安装，参考https://pandoc.org/installing.html。

## 修改配置

### 修改.gitconfig

找到git的安装路径，打开配置文件gitconfig (如d:\Program Files\Git\etc)，加入以下内容:

1. 添加文件名中文支持

[core]

    quotepath = false 
    
2. wdiff比较word

[diff "pandoc"]

    textconv=pandoc --to=markdown
    
    prompt = false
    
[alias]

    wdiff = diff --word-diff=color --unified=1

### 增加.gitattributes文件

在**项目的根目录**下里新增一个.gitattributes文件，并进行设置。如果不想让这些属性文件与其他文件一同提交，也可以在.git/info/attributes文件进行设置

  *.doc diff=pandoc 
  
  *.docx diff=pandoc 
  
## git配置用户名和邮箱

在项目目录或工作目录下右击→Git Bash Here

git config --global user.name  "username" 

git config --global user.email  "email"
//这一步完成后会在前面的 gitconfig文件夹中出现你的配置信息？

## 工程目录git初始化

git init  //初始化

也可在RStudio新建project时就勾选

## 简单操作

git add file.doc  //暂存指定文件

git add . //暂存全部文件

git commit -am "版本标识符" //版本标号，建议加上日期

**git wdiff** file.docx         //查看当前改动

git log                     //查看历史版本

git log -p --word-diff=color file.docx //查看 历次的改变（all changes）

git reflog

git reset --hard vesion     //版本回退

git status                  //查看当前数据

pandoc -s file.docx -t markdown -o file.md  //pandoc强大之处，可直接进行文件转换；这里是将.docx转换为.md文件，在相应的项目目录下会多出一个markdown文件

其他（略）