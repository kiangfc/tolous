---
title: git guide
author: atien
date: '2021-01-31'
slug: git-guide
categories:
  - git
tags: []
---


## 安装与配置git

在windows上安装git后，因为Git是分布式版本控制系统，所以需要填写用户名（atien）和邮箱（example@126.com）作为一个标识。
在开始菜单找到"Git --> Git Bash"或在项目根目录文件夹里右键弹出 Git Bash，输入

$ git config --global user.name "atien"

$ git config --global user.email "example@126.com"

注意：git config --global 参数，有了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱。

## 创建版本库repository

$ cd e:

$ cd casst

$ cd github

$ mkdir casst

$ cd casst

$ pwd #显示当前目录


### 初始化版本库

通过命令 git init 把这个目录（**项目根目录**）变成git可以管理的仓库

$ git init

在版本库casst目录下新建测试文件git.md。

### 使用命令 git add git.md添加到暂存区里面去

$ git add git.md

### 用命令 git commit告诉Git，把文件提交到仓库。

$ git commit -m 'git.md提交'

可以通过命令git status来查看是否还有文件未提交。若提交后继续来修改git.md内容，则继续使用git status来查看未被提交的修改。

$ git status

把修改后的git.md文件**添加**到版本库（$ git add git.md）

查看git.md文件到底改了什么内容

$ git diff git.md

重新提交到仓库，提交修改和提交文件是一样的2步(第一步是git add 第二步是：git commit)

$ git commit -m "文件增加git status及git diff内容"

注意：

1. 显示工作区和暂存区的不同

输出工作区和暂存区的 different (不同)。

git diff

还可以显示本地仓库中任意两个 commit 之间的文件变动：

git diff <commit-id> <commit-id>

2. 显示暂存区和最近版本的不同

输出暂存区和本地最近的版本 (commit) 的 different（不同）。

git diff --cached

3. 显示暂存区、工作区和最近版本的不同

输出工作区、暂存区 和本地最近的版本 (commit) 的 different (不同)。

git diff HEAD


## 版本回退

继续对git.md文件进行修改

$ git add git.md

$ git commit -m "添加git.md内容#版本回退"。查看历史

$ git log

$ git log –pretty=oneline 

把当前的版本回退到上一个版本

$ git reset --hard HEAD^

回退到上上个版本

$ git reset --hard HEAD^^
 
如果要回退到前n个版本

$ git reset --hard HEAD~n 

回退（恢复）最新版本

$ git reset --hard （最新）版本号

而版本号可以用命令获取

$ git reflog

如6fcfc89，则回退（恢复）最新版本

git reset --hard 6fcfc89

## 理解工作区与暂存区

工作区：就是你在电脑上看到的目录，比如目录下testgit里的文件(.git隐藏目录版本库除外)。或者以后需要再新建的目录文件等等都属于工作区范畴。

版本库(Repository)：工作区有一个隐藏目录.git,这个不属于工作区，这是版本库。其中版本库里面存了很多东西，其中最重要的就是stage(暂存区)，还有Git为我们自动创建了第一个分支master,以及指向master的一个指针HEAD。

使用Git提交文件到版本库有两步：

1. 是使用 git add 把文件添加进去，实际上就是把文件添加到暂存区。

2. 使用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支上。

## Git撤销修改和删除文件操作

### 撤销修改

未提交之前，发现内容有误，马上恢复以前的版本，如下几种方法可以做修改：

1. 如果我知道要删掉那些内容的话，直接手动更改去掉那些需要的文件，然后add添加到暂存区，最后commit掉。

2. 我可以按以前的方法直接恢复到上一个版本。使用 git reset --hard HEAD^

3. 直接使用撤销命令(把git.md文件在工作区做的修改全部撤销)

$ git checkout -- git.md

- git.md自动修改后，还没有放到暂存区，使用撤销修改就回到和版本库一模一样的状态。

- 另外一种是git.md已经放入暂存区了，接着又作了修改，撤销修改就回到添加暂存区后的状态。

test1

### 删除文件

添加一个文件git2.md,然后提交

$ git add git2.md

$ git commit -m "添加git2.md文件"

删除文件

直接在文件目录中把文件删了，或使用命令

$ rm git2.md

彻底从版本库中删掉了此文件的话，可以再执行commit命令提交掉。

只要没有commit之前，如果想在版本库中恢复git2.md文件，执行命令

$ git checkout -- git2.md

## 远程仓库

第一步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果有的话，直接跳过此如下命令，如果没有的话，打开命令行，输入如下命令：

$ ssh-keygen -t rsa –C “youremail@example.com”, 由于我本地此前运行过一次，所以本地有。id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第二步：登录github,打开” settings”中的SSH Keys页面，然后点击“Add SSH Key”,填上任意title，在Key文本框里黏贴id_rsa.pub文件的内容。
点击 Add Key，你就应该可以看到已经添加的key。

### 如何添加远程库

现在的情景是：我们已经在本地创建了一个Git仓库后，又想在github创建一个Git仓库，并且希望这两个仓库进行远程同步，这样github的仓库可以作为备份，又可以其他人通过该仓库来协作。

首先，登录github上(example)，然后在右上角找到“create a new repo”创建一个新的仓库casst。
目前，在GitHub上的这个testgit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

根据GitHub的提示，在本地的casst仓库下运行命令

$ git remote add origin https://github.com/example/casst.git

或

$ git remote add origin git@github.com:example/ips.git

把本地库的内容推送到远程，使用 git push命令，实际上是把当前分支master推送到远程。

$ git push -u origin master

由于远程库是空的，我们第一次推送master分支时，加上了 –u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。推送成功后，可以立刻在github页面中看到远程库的内容已经和本地一模一样了，上面的要输入github的用户名和密码如下所示

从现在起，只要本地作了提交，就可以通过如下命令：

$ git push origin master

把本地master分支的最新修改推送到github上了，现在你就拥有了真正的分布式版本库了。

### 如何从远程库克隆

上面我们了解了先有本地库，后有远程库时候，如何关联远程库。

现在我们想，假如远程库有新的内容了，我想克隆到本地来 如何克隆呢

首先，登录github，创建一个新的仓库，名字叫casst

现在，远程库已经准备好了，下一步是使用命令git clone克隆一个本地库了

$ git clone  https://github.com/example/casst.git

接着在本地目录下 生成casst目录了。

## 创建与合并分支

在 版本回填退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即master分支。HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
首先，我们来创建dev分支，然后切换到dev分支上。如下操作：

$ git checkout -b dev

git checkout命令加上 –b参数表示创建并切换，相当于如下2条命令

$ git branch dev

$ git checkout dev

git branch查看分支，会列出所有的分支，当前分支前面会添加一个星号。然后我们在dev分支上继续做
test dev

现在dev分支工作已完成，现在我们切换到主分支master上，继续查看readme.txt内容如下

$ git checkout master

$ cat git.md

现在我们可以把dev分支上的内容合并到分支master上了，可以在master分支上，使用如下命令 git merge dev 如下所示：

$ git merge dev

git merge命令用于合并指定分支到当前分支上，合并后，再查看readme.txt内容，可以看到，和dev分支最新提交的是完全一样的。

注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。
合并完成后，我们可以接着删除dev分支了

$ git branch -d dev

总结创建与合并分支命令如下：


1. 查看分支：git branch

2. 创建分支：git branch name

3. 切换分支：git checkout name

4. 创建+切换分支：git checkout –b name

5. 合并某分支到当前分支：git merge name

6. 删除分支：git branch –d name

## 如何解决冲突？

新建一个新分支，比如名字叫fenzhi1，在git.md添加1行内容，然后提交。

$ git checkout -b fenzhi1

$ git add git.md

$ git commit -m "添加解决冲突新建分支1行内容"

现在切换到master分支上来，也在也在最后一行添加内容，如上述3个命令及以下切换、提交命令

$ git checkout master

$ git add git.md

$ git commit -m "在master分支上添加解决冲突新建分支、切换等命令内容"。

现在在master分支上来合并fenzhi1，如下操作：

$ git merge fenzhi1

可查看状态

$ git status

查看git.md内容

$ cat git.md

Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，其中<<<HEAD是指主分支修改的内容，>>>>>fenzhi1 是指fenzhi1上修改的内容
可以修改成和主干上代码一样后保存

$ git add git.md

$ git commit -m "conflict fixed"

查看分支合并情况，使用命令git log

$ git log

## 分支管理策略

通常合并分支时，git一般使用”Fast forward”模式，在这种模式下，删除分支后，会丢掉分支信息，现在我们来使用带参数 –no-ff来禁用”Fast forward”模式。首先我们来做demo演示下：

1. 创建一个dev分支。

$ git checkout -b dev

2. 修改readme.txt内容。

3. 添加到暂存区。

$ git add git.md

$ git commit -m "add merge"

4. 切换回主分支(master)。

$ git checkout master

5. 合并dev分支，使用命令 git merge –no-ff -m “注释” dev

$ git merge --no-ff -m "merge with no-ff" dev

6. 删除dev分支

$ git branch -d dev

查看分支

$ git branch

7. 查看历史记录

$ git log --graph --pretty=oneline --abbrev-commit

分支策略：首先master主分支应该是非常稳定的，也就是用来发布新版本，一般情况下不允许在上面干活，干活一般情况下在新建的dev分支上干活，干完后，比如上要发布，或者说dev分支代码稳定后可以合并到主分支master上来。

## bug分支

在开发中，会经常碰到bug问题，那么有了bug就需要修复，在Git中，分支是很强大的，每个bug都可以通过一个临时分支来修复，修复完成后，合并分支，然后将临时的分支删除掉。

比如我在开发中接到一个404 bug时候，我们可以创建一个404分支来修复它，但是，当前的dev分支上的工作还没有提交。

并不是我不想提交，而是工作进行到一半时候，我们还无法提交，比如我这个分支bug要2天完成，但是我issue-404 bug需要5个小时内完成。怎么办呢？还好，Git还提供了一个stash功能，可以把当前工作现场 ”隐藏起来”，等以后恢复现场后继续工作。


$ git stash

首先我们要确定在那个分支上修复bug，比如我现在是在主分支master上来修复的，现在我要在master分支上创建一个临时分支

$ git checkout -b issue-404

修改git.mdde 的内容后提交

$ git add git.md

$ git commit -m "fix bug 404"

修复完成后，切换到master分支上，并完成合并，最后删除issue-404分支。

$ git checkout master

$ git merge --no-ff -m "merge bug fix 404" issue-404

$ cat git.md

$ git branch -d issue-404

现在回到dev分支上干活了

$ git checkout dev

使用命令 git stash list来查看工作现场

$ git stach list

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，可以使用如下2个方法：

1. git stash apply恢复，恢复后，stash内容并不删除，你需要使用命令git stash drop来删除。

$ git stash list

$ git stash drop

2. 另一种方式是使用git stash pop,恢复的同时把stash内容也删除了。

$ git stash list

$ git stash pop

## 多人协作

当你从远程库克隆时候，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且远程库的默认名称是origin。

1. 要查看远程库的信息 使用 

$ git remote

2. 要查看远程库的详细信息 使用 

$ git remote –v

### 推送分支：

推送分支就是把该分支上所有本地提交到远程库中，推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上： 使用命令 

$ git push origin master

如果我们现在要推送到其他分支，比如dev分支上，我们还是那个命令 

$ git push origin dev

那么一般情况下，那些分支要推送呢？master分支是主分支，因此要时刻与远程同步。
一些修复bug分支不需要推送到远程去，可以先合并到主分支上，然后把主分支master推送到远程去。

### 抓取分支

多人协作时，大家都会往master分支上推送各自的修改。现在我们可以模拟另外一个同事，可以在另一台电脑上（注意要把SSH key添加到github上）或者同一台电脑上另外一个目录克隆，新建一个目录名字叫casst2
但是我首先要把dev分支也要推送到远程去

$ git push origin dev

接着进入casst2目录，进行克隆远程的库到本地来

$ git clone https://github.com/example/casst

现在我们的小伙伴要在dev分支上做开发，就必须把远程的origin的dev分支到本地来，于是可以使用命令创建本地dev分支

$ git checkout -b dev origin/dev

现在小伙伴们就可以在dev分支上做开发了，开发完成后把dev分支推送到远程库时。

$ git add git.md

$ git commit -m "git.md 增加xxx内容"

$ git push origin dev

小伙伴们已经向origin/dev分支上推送了提交，而我在我的目录文件下也对同样的文件同个地方作了修改，也试图推送到远程库时

$ git add git.md

$ git commit -m "git.md 增加xxx内容"

$ git push origin dev

推送失败，因为我的小伙伴最新提交的和我试图推送的有冲突，解决的办法也很简单，上面已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后在本地合并，解决冲突，再推送。

$ git pull

git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接

$ git branch --set-upstream dev origin/dev

$ git pull

这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的 解决冲突完全一样。解决后，提交，再push

现在手动已经解决完了，我接在需要再提交，再push到远程库里面去。如下所示：

$ git add git.md

$ git commit -m "merge & fix git.md"

$ git push origin dev

因此：多人协作工作模式一般是这样的：

首先，可以试图用git push origin branch-name推送自己的修改.

如果推送失败，则因为远程分支比你的本地更新早，需要先用git pull试图合并。

如果合并有冲突，则需要解决冲突，并在本地提交。再用git push origin branch-name推送。














