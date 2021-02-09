---
title: '升级R从3.6.3到4.0.3'
date: '2021-01-28 10:15:47'
---

问题：**R从3.x升级到4.x，原R包需要用Rtools4.0+重新编译。**

## 安装Rtools4.0

1. 安装Rtools4.0到d:\Program files\R\rtools40\（用于编译R 4.0之前的packages）

2. 把d:\Program files\R\rtools40\、d:\Program files\R\rtools40\mingw64\bin\、d:\Program files\R\rtools40\usr\bin\添加到系统环境变量的path里（测试：Sys.which("make")，应出来"d:\\Program files\\R\\rtools40\\usr\\bin\\make.exe" ）

3. 把.\etc\pacman.d中
在mirrorlist.mingw32文件开头添加：Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/i686/；
在mirrorlist.mingw64文件开头添加：Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/mingw/x86_64/：
在mirrorlist.msys文件开头添加：Server = https://mirrors.tuna.tsinghua.edu.cn/msys2/msys/$arch/

## 升级R

1. 下载最新版R for windows的base：R-4.0.3-win.exe安装到d:\Program files\R\R-4.0.3\

2. 在.\R-4.0.3\etc\的Rprofile.site的最后加：options(repos=structure(c(CRAN="https://mirrors.tuna.tsinghua.edu.cn/CRAN/")))；

## 启动R-4.0.3Gui

运行如下命令：

1. 检查确认安装包库路径 

.libPaths()

2. 获取旧包名称（如位于D:/Program Files/R/R-3.6.3/library）

old_packages <- installed.packages(lib.loc =  "D:/Program Files/R/R-3.6.3/library")

old_packages <- as.data.frame(old_packages)

list.of.packages <- unlist(old_packages$Package)

3. 删除旧R包

remove.packages( installed.packages( priority = "NA" )[,1] )

4. 重新安装所有R包

new.packages <- list.of.packages[!(list.of.packages %in% installed.packages()[,"Package"])]

if(length(new.packages)) install.packages(new.packages)
lapply(list.of.packages,function(x){library(x,character.only=TRUE)})

弹出Question:
Do you want to install from sources the packages which need compilation?
选择“是”。

5. 更新R包

启动RStudio，选择tools-global options-packages-TUNA镜像，然后选择择tools-check for packages updates

*R语言无法调用stats.dll的问题解决---unable to load shared object ‘d:/Program files/R/R-4.0.3/library/stats/libs/x64/stats.dll
将d:\Program files\R\R-3.6.3\bin\x64\目录下的所有文件，拷贝到d:\Program files\R\R-4.0.3\library\stats\libs\x64\。*

6. 安装github包

有些包来自于github，则用install_github安装，如
remotes::install_github("ricardo-bion/ggradar",  dependencies = TRUE)，当然也可用devtools、pacman等的安装命令。