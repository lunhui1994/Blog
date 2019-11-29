---
title: Git入门篇
date: 2019-08-15 15:14:27
categories: Git
tags: Git
keywords: Git, Git入门
---

##### 开始

 - 首先我们需要去github官网申请git账号。[git官网](https://github.com/)
 - 申请之后，我们进入自己的linux服务器

<!-- more -->
```
    // 生成key
    ssh-keygen
    //查看公钥  
    cat ~/.ssh/id_rsa.pub
```

- 然后在GitHub上加入这个公钥 [配置公钥](https://github.com/settings/keys) 

![github-key](/img/github-key.jpg)

- 设置git命令的简写模式（alias）

```
    git config --global alias.co checkout
    git config --global alias.br branch
    git config --global alias.ci commit
    git config --global alias.st status
```
- 设置自己的名字和邮箱

```
    git config --global user.name "your name"
    git config --global user.email "your email @.com"
```
- ##### fork

如果你想使用别人的项目，就需要fork。
![github-fork](/img/github-fork.jpg)

* 一般我们参与公司项目，都会先fork公司的仓库。


- ##### clone

进入一个你想存放项目的文件夹。
![github-clone](/img/github-clone.jpg)

如图复制ssh地址。

* 一般我们复制的这个ssh地址，是先fork了公司的仓库，然后回到自己的仓库下面复制自己的ssh。

```
    git clone git@github.com:xxx/xxx.git
```
然后就可以对代码进行修改和提交了。



- ##### checkout

项目有不同的分支。
一般本地主分支为master。
自己的远程仓库为 origin
如果是公司的项目，可能还会添加一个公司仓库 gongsi

可以用以下命令查看。
```
    git br -va
```

一般我们都master分支上。

那么当我们修改了master分支的文件，但是又想恢复它到我们修改之前的状态，就需要checkout

```
    git co xxx.html
```

checkout也可以创建本地分支: 

origin/develop为远程仓库origin里面的一条分支，

我们要在本地创建一条和它一样的分支。

```
    git co -b develop origin/develop
```

另一种情况 本地和远程都只有master分支，

我们要在master的基础上添加订制功能，

需要独立出来一条和master一样的分支，然后再修改。

```
    git co -b develop origin/master

    # 增加完新功能之后

    git push origin develop
```

这样就会在本地和远程origin都创建了一条develop分支完成定制功能的添加又不影响原来的master分支。

- ##### commit

当我们修改某个文件，使用git st就可以看到哪些文件被修改了。
然后使用 git add 可以将我们修改过的文件添加进暂存区
```
    git st

    git add xxx.html
```

commit为某次修改的描述，是阶段性的。
我们每完成一个功能，或者每修复一个bug，最好都进行一次提交。

```
    git ci -m '描述'
```
最后将代码push到我们的远程分支

```
    git push origin master(分支名)
```

- ##### reset
有时候我们会后悔添加了文件。那么可以用reset 返回

```
    git reset xxx.html
```

这样就可以返回add之前的文件状态。

同样的我们如果已经push到远程分支了

想要回到我们push之前的状态，或者再之前的某个版本。

```
    git reflog
    git reset --hard 版本号
```
以上两个步骤，第一步是查看我们这个分支的所有版本号。

复制你想要回退的版本号，然后执行第二步，就会回退到目标版本了。

然后再次执行你想要add，ci，push等命令，将你想要提交的文件push到远程。

- ##### fetch/merge

push之后我们的远程分支就会和本地分支的内容一样了。

但是如果我们是一个公共项目，那就需要并入公共仓库（gongsi）。

这个就需要管理员来操作了。

![github-pull1](/img/github-pull1.jpg)

![github-pull2](/img/github-pull2.jpg)

等管理员合并了之后，别人就需要fetch并且merge你的代码，以此来使大家的代码都是同步的。

```
    git fetch --all

    git merge gongsi/master
```

每次push之前我们都应该先merge一下公共仓库的代码。
以免我们在旧代码上修改提交导致冲突。

- ##### delete

那么我们如何删除自己的本地分支和远程分支呢，拿new_master举例

```
    git br -d new_master
    git push origin -d new_master
```

- ##### remote

当我们从某个git地址clone下来仓库后，仓库的远程地址就是你所clone的地址。

此时如果我们想修改我们的远程仓库怎么办呢（也就是修改origin的远程地址）

那就用到remote了

首先查看远程地址：
```
    git remote -v
```

然后修改远程地址

```
    git remote set-url [仓库名称] [url]
    
    # 例如：修改origin这个仓库的远程地址。
    # git remote set-url origin git@github.com:xiaoming/project.git
```

那么如果要新添加一个远程地址呢？比如我们添加公司的（或者其他任何）。

```
    git remote add [自定义远程仓库名] [url]

    # 例如：
    # git remote add gongsi git@github.com:gongsi/project.git
```

添加完公司的仓库之后，我们远程公司的仓库有了，但是本地还没有，怎么办呢，就用到前面的checkout了。

```
    git co -b [创建本地分支名] [远程仓库名/远程仓库分支名]

    # 例如：创建一个本地分支 develop 该分支的内容和gongsi/develop分支的内容一致。
    # git co -b develop gongsi/develop

```

推送到自己的远程分支。
如果我们的origin上没有 develop 分支，那就会自动创建一个。

这样就保持三个分支一致了。

```
    git push origin develop

```


- ##### stash

有的时候，我们正在修改master分支。突然有一个紧急需求需要在develop上修改。

但是master还没有修改完，我们不能切换分支。

那怎么办呢？我们可以使用stash。

```
    git stash

    git co develop

    git co master

    git stash pop
```
以上三个步骤

第一步 将我们修改的内容缓存起来

第二步 切换到develop 分支,然后修改提交之后

第三步 切换到master分支

第四步 恢复切换到develop之前的master分支的修改内容。

以上就是常用的git命令，当然还有更多的和更深的命令，可以扩展了解一下。


