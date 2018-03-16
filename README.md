# 目录
[1. Git 简介](#1)   
[2. 基本命令](#2)   
        [2.1. git config: 环境设置命令](#2.1)  
        [2.2. git init: 初始化本地仓库](#2.2)   
        [2.3. git clone: 克隆远程仓库到本地](#2.3)   
        [2.4. ssh-keygen: 生成SSH公钥](#2.4)   
        [2.5. git status 查看当前状态](#2.5)   
        [2.6. git add: 添加到暂存区](#2.6)   
        [2.7. git commit: 提交到版本库](#2.7)   
        [2.8. git log: 查看提交历史](#2.8)   
        [2.9. git diff: 显示变更内容](#2.9)   
        [2.10. git reset: 回退版本](#2.10)   
        [2.11. git push: 推送本地分支到远程](#2.11)   
        [2.12. git pull: 拉取远程分支到本地并合并](#2.12)   
[3. 分支管理](#3)   
        [3.1. git branch: 分支操作](#3.1)   
        [3.2. git checkout: 分支间切换](#3.2)   
        [3.3. git merge: 合并分支](#3.3)   
        [3.4. git rebase: 分支的变基](#3.4)   
        [3.5. git cherry-pick: 挑拣节点合并到当前分支上](#3.5)   
[4. 其他命令](#4)   
        [4.1. git revert: 撤销某次提交](#4.1)   
        [4.2. git tag: 标签的操作](#4.2)   
        [4.3. git show: 显示信息](#4.3)   
        [4.4. git blame: 查看文件每行的提交历史（追责）](#4.4)   
[5. 团队协作应用](#5)   
        [5.1 分支设置](#5.1)   
        [5.2 普通开发人员的操作](#5.2)   
        [5.3 团队 leader 的操作](#5.3)   
[6. 结束语](#6)   

---

<a name="1"></a>   
# 1. Git 简介 
`Git 的诞生：`
> Linus(Linux之父)花了两周时间自己用C写了一个分布式版本控制系统，这就是Git！   
一个月之内，Linux系统的源码已经由Git管理了！

`几个概念：`
> `工作区`、`版本库`、`暂存区`   
如下图所示：

![image](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/1.png)

> **1.** 图中左侧为工作区，右侧为版本库。在版本库中标记为`index`的区域是暂存区（`stage`,`index`），标记为`master`的是`master`分支所代表的目录树。

> **2.** 图中我们可以看出此时`HEAD`指针实际是指向`master`分支的一个“游标”。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。

> **3.** 图中的`objects`标识的区域为`Git`的对象库，实际位于`.git/objects`目录下。

> **4.** 当对工作区修改（或新增）的文件执行`git add`命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的`ID`被记录在暂存区的文件索引中。

> **5.** 当执行提交操作`git commit`时，暂存区的目录树写到版本库（对象库）中，`master`分支会做相应的更新。即`master`指向的目录树就是提交时暂存区的目录树。

> **6.** 当执行`git reset HEAD`命令时，暂存区的目录树会被重写，被`master`分支指向的目录树所替换，但是工作区不受影响。

> **7.** 当执行`git rm --cached <file>`命令时，会直接从暂存区删除文件，工作区则不做出改变。

> **8.** 当执行`git checkout .`或者`git checkout -- <file>`命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。

> **9.** 当执行`git checkout HEAD .`或者`git checkout HEAD <file>`命令时，会用`HEAD`指向的`master`分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

<a name="2"></a>   
# 2. 基本命令 

<a name="2.1"></a>   
## 2.1. git config: 环境设置命令 

通常情况下，安装完Git后的第一件事就是设置**用户名称**和**邮件地址**。每一个Git的提交都会使用这些信息，如果不设置则无法进行提交。

`$ git config --global user.name "goto456"    // 设置用户名称`   
`$ git config --global user.email "goto456@126.com"    // 设置邮件地址`

> 使用`--global`参数表示设置了全局的环境，如果想对与特定的项目使用不同的用户名和邮件地址，则可已在该项目目录下不使用`--global`参数设置不同的用户名和邮件地址。

`git config --list`命令可以列出当前Git所有的配置信息。

<a name="2.2"></a>   
## 2.2. git init: 初始化本地仓库

获取一个 Git 仓库有2中方法：
> 1. 本地初始化一个仓库
> 2. 从远程克隆一个仓库到本地

对于第1种方式，如果想对本地现有的一个项目用 Git 来管理，可以直接进入该项目的目录下执行如下命令，就可以将其初始化成一个 Git 仓库了。   
`$ git init`

<a name="2.3"></a>   
## 2.3. git clone: 克隆远程仓库到本地

对于第二种方式，也是最常用的方式，比如你在 GitHub 上（或者其他代码托管网站）已经建立了一个项目，你就可以将该项目从远程克隆到本地，这就有了该项目在本地的 Git 仓库。

`$ git clone git@github.com:goto456/leetcode.git // 通过ssh方式克隆`   
`$ git clone https://github.com/goto456/leetcode.git // 通过https方式克隆`

以上2种方式有如下区别：
> **1. https方式**：不管是谁，只要拿到该项目的 url 可以随便 clone，但是在 push 到远程的时候需要验证用户名和密码；   
> **2. ssh方式**：需要现将你电脑的SSH key（SSH公钥）添加到GitHub（或者其他代码托管网站）上，这样在 clone  项目和 push 项目到远程时都不需要输入用户名和密码。   

如何生成`SSH key`，参见下一条命令：ssh-keygen

<a name="2.4"></a>   
## 2.4. ssh-keygen: 生成SSH公钥

生成公钥之前先检查系统中是否已经有了公钥，公钥一般在`~/.ssh/`目录下。如果该目录下存在`id_rsa.pub`文件，这就是公钥（id_rsa 文件是私钥）；如果不存在此文件，甚至连`.ssh`目录都不存在，则需要用 ssh-keygen 命令来生成。如下所示：

`$ ssh-keygen // 然后一直按回车键`

这就可以生成 SSH key 了，公钥`id_rsa.pub`文件的内容大致如下所示：
> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC/bdrJjo6HNCuzhpTZNVsTju5NnksTca2kRzfbZlFs+8S4bs8hTlo6aV/GJPkTT0zxJfuOgfd4B4O8Xh0NQg51Zl4yCsFdnIKFA0tgBOg/H5oi48Ypoo8h3LO+S8gd8HR5eqndjINx3kCrQx4gISCW25d7GKTm3c40YPoUCIL0Zg4+iuu2ioeVCpv+FSCKhlcrYew7c2aEe3p/oOmATT3UX9S4t94qzy8KkxRlnQ4a8zQ3OTRE23r7+PPHGN8oBAen//NQNn9COUhqU3pHfvtxqYrG1Blhf0t6HhkHz9Fd8StCUIDQv7yHHvuuPFumh3/8PNG8DDPQqWsqXmK2XozT goto456@126.com

<a name="2.5"></a>   
## 2.5. git status 查看当前状态
可以在任何时候使用该命令查看当前的状态。一般会显示当前所处的分支，以及当前工作区是否干净，是否有文件要提交。   
当你克隆远程仓库到本地后，通过该命令查看当前状态时，显示信息如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-2.png)

当你修改了一个文件后，再用该命令查看当前状态，显示信息如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-3.png)

它会提示你把当前的变更添加到暂存区或者丢弃该变更。   

当你新增一个文件后，用该命令查看当前状态，显示信息如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-4.png)

它会显示新增的文件状态是未跟踪，并且提示用`git add`命令将其添加到暂存区。

<a name="2.6"></a>   
## 2.6. git add: 添加到暂存区
无论你新增了一个文件或者对已有的文件进行了修改，都需要将其添加到暂存区，然后提交到版本库。

```
$ git add hello.txt      //后面接文件名，表示将某个文件添加到暂存区   
$ git add .                 //后面接一个点，表示将全部文件添加到暂存区
```

分别执行上述2条命令后，如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-5.png)

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-6.png)

将新增/修改的文件添加到暂存区后，并未提交到版本库，还需要通过`git commit`命令提交到版本库。

<a name="2.7"></a>   
## 2.7. git commit: 提交到版本库
该命令是将添加到暂存去的变更提交到版本库，主要有一下几个常用的：

```
$ git commit -m "变更的说明信息"       //标准用法，提交到版本库并填写相关说明信息
$ git commit -am "变更的说明信息"     //加上-a参数，则不需要上一步的git add过程，可以直接将修改过的变更直接提交到版本库，但不会把新增的文件提交
```

分别执行上述2条命令后，如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-7.png)

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-8.png)

**注：第2条命令一般在不想把新增的文件提交到版本库时用的比较多**

执行完git commit命令提交到版本库后，一次简单的流程就算走完了。接下来可以通过git log命令来查看所有的提交历史。

<a name="2.8"></a>   
## 2.8. git log: 查看提交历史
如果发现提交了一个错误版本，想回退到上个版本时，就可以通过该命令来查看所有的历史版本，以选择回退到历史的哪个版本。

```
$ git log                  //查看历史版本
$ git log --graph     //以图形的方式查看历史（这个我比较常用，能很好的显示各个分支之间的关系）
```

执行后，如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-9.png)

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-10.png)

可以看出第2个图的左边显示出了分支的图形

<a name="2.9"></a>   
## 2.9. git diff: 显示变更内容
当你对文件进行了修改，想查看进行了哪些修改时，可以通过该命令查看。   
`git diff`命令会显示修改的文件中哪些内容进行了修改，包括新增了哪些内容，删除了哪些内容。

```
$ git diff                            //后面不接参数，表示显示所有修改文件的变更
$ git diff README.md      //后面接文件名，表示只显示该文件的变更
```
例如：我对 `README.md` 文件进行了修改，删除了1行，新增了2行，然后用该命令查看进行了哪些修改，如下图所示： （`“+”` 表示新增的内容，`“-”` 表示删除的内容）   

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-11.png)

<a name="2.10"></a>   
## 2.10. git reset: 回退版本
常用于回退版本或者在各个版本间进行跳跃切换。
我们可以先用 `git log` 看一下当前历史版本，如下图所示：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-12.png)

如果要回退到前一个版本，则只需要输入：`git reset HEAD~1`，执行完再看一下历史版本：   

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-13.png)   
我们已经回退到前一个版本了。

如果需要回退到前2个版本，命令是：`git reset HEAD~2`，回退到前n个版本就是：`git reset HEAD~n`   
**注意：在 Git 中，用 `HEAD` 表示指向当前版本的指针**

如果需要回退到任何一个版本，则需要替换成该版本的 `commit id` 就可以了，例如：`git reset a8336834b50daafa0793370`，执行完再看一下历史：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-14.png)   
发现我们已经回退到该版本了。

**一般情况下会加一个 `--hard` 参数：`git reset --hard HEAD~1` 或 `git reset --hard a8336834b50daafa0793370`，表示回退到某个版本并且丢弃调工作区进行的修改，而不加该参数表示回退到某个版本但保留工作区的修改。**

<a name="2.11"></a>   
## 2.11. git push: 推送本地分支到远程
当修改完成，本地的改动都已经提交到本地库，则可以将本地分支推送到远程代码库了。 

命令：`git push origin master`   
`origin` 表示远程代码库的一个别名（也可以修改为其他名字，可通过 `git remote` 修改），`master` 表示需要推送的分支名称。

如果，push 的过程中提示当前分支进度落后于远程的分支，则需要通过 `git pull` 命令来拉取远程最新状态和本地分支进行合并，完成之后再 push 到远程就可以了。

<a name="2.12"></a>   
## 2.12. git pull: 拉取远程分支到本地并合并
一般是本地分支的进度落后于远程分支时，需要使用该命令。

命令：`git pull origin master`   
`origin` 表示远程代码库的一个别名（也可以修改为其他名字，可通过 `git remote` 修改），`master` 表示需要拉取合并的分支名称。

**常用 `git pull --rebase origin master` 用 rebase 的方式进行，不会产生 merge 保持分支干净、整洁**

<a name="3"></a>   
# 3. 分支管理

<a name="3.1"></a>   
## 3.1. git branch: 分支操作
命令：`git branch` 用于显示本地所有分支以及当前所在哪个分支。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-15.png)   
图中显示本地有 `master` 和 `dev` 两个分支，并且正处在 `master` 分支上。

命令：`git branch branchName` 用于新建名为 branchName 的新分支。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-16.png)   
图中新建了一个名为 test 的新分支。

命令：`git branch -d branchName` 用于删除名为 branchName 的分支。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-17.png)    
图中删除了一个名为 test 的分支。

命令：`git branch -D branchName` 用于强制删除分支。

<a name="3.2"></a>   
## 3.2. git checkout: 分支间切换
该命令除了进行分支间切换功能外，还可以用来丢弃工作区中的修改内容，这里就不作介绍了，仅介绍分之间的切换功能。

命令：`git checkout branchName` 用于从当前分支切换到名为 branchName 的分支上。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-18.png)   
图中先显示在 master 分支上，后来切换到了 dev 分支上。

命令：`git checkout -b branchName` 用于新建名为 branchName 的分支并切换到该分支上。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-19.png)   
图中新建了 test 分支并切换到了该分支上。   
**注意与 `git branch` 新建分支的区别，此处除了新建分支外还进行了切换操作**

<a name="3.3"></a>   
## 3.3. git merge: 合并分支
该命令用于合并两个分支。   
命令：`git merge branchName` 用于将名为 branchName 的分支合并到当前分支。   
有两种合并方式：
1. fast-forward 方式合并：   
命令：`git merge dev` 

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-20.png)

2. 非fast-forward 方式合并：   
命令：`git merge dev` 

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-21.png)

**注意两种方式的区别：fast-forward 方式仅仅是移动了 HEAD 指针，而非 fast-forward 方式则是新建了一个节点**

<a name="3.4"></a>   
## 3.4. git rebase: 分支的变基
命令：`git rebase master` 将当前分支 rebase 到 master 分支   
命令：`git rebase master dev` 将 dev 分支 rebase 到 master 分支

这种操作很难用语言解释，我用一个图来说明其与 merge 操作的区别：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-22.png)   
由图可知，**rebase 操作相当于将 C3 节点拿下来换了一个位置重新放置**。最后不会产生合并的痕迹，所有分支都是同一条直线。

**我们来看一个详细的例子：**   
 你从 master 分支的 C2 上创建了一个新分支 server，为服务端添加了一些功能，提交了 C3 和 C4。 然后从 C3 上创建了特性分支 client，为客户端添加了一些功能，提交了 C8 和 C9。 最后，你回到 server 分支，又提交了 C10。
 
![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-23.png)   

假设你希望将 client 中的修改合并到主分支并发布，但暂时并不想合并 server 中的修改，因为它们还需要经过更全面的测试。这时，你就可以使用 git rebase 命令的 --onto 选项，选中在 client 分支里但不在 server 分支里的修改（即 C8 和 C9），将它们在 master 分支上重放：   

命令：`git rebase --onto master server client`   
以上命令的意思是：“取出 client 分支，找出处于 client 分支和 server 分支的共同祖先之后的修改，然后把它们在 master 分支上重放一遍”。 这理解起来有一点复杂，不过效果非常酷。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-24.png)

现在可以快进合并 master 分支了。（如下图 快进合并 master 分支，使之包含来自 client 分支的修改）：

`$ git checkout master`   
`$ git merge client`

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-25.png)

接下来你决定将 server 分支中的修改也整合进来。 使用 git rebase master server 命令可以直接完成，这样做能省去你先切换到 server 分支，再对其执行变基命令的多个步骤。

`$ git rebase master server`   
如下图 将 server 中的修改变基到 master 上，server 中的代码被“续”到了 master 后面。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-26.png)

然后就可以快进合并主分支 master 了：

`$ git checkout master`   
`$ git merge server`   
至此，client 和 server 分支中的修改都已经整合到主分支里了，你可以删除这两个分支，最终提交历史会变成下图的样子，非常干净、整洁：

`$ git branch -d client`   
`$ git branch -d server`   

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-27.png)

<a name="3.5"></a>   
## 3.5. git cherry-pick: 挑拣节点合并到当前分支上
该命令一般用于从其他分支上挑拣某些节点到当前分支。   
命令：`git cherry-pick commit_id`

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-28.png)   
上图中我想把 ruby_client 分支上的 e43a6 这个节点合并到 master 分支上，但不需要 5ddae 这个节点，那么我们就可以使用下面的命令：   
`$ git checkout master` 先切换到 master 分支   
`$ git cherry-pick e43a6` 将 e43a6 节点挑拣合并到当前分支   
完成后如下图所示：   

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-29.png)   

**注意：该节点被挑拣合并到 master 上后会产生一个新的节点**

<a name="4"></a>   
# 4. 其他命令

<a name="4.1"></a>   
## 4.1. git revert: 撤销某次提交
该命令用于撤销历史上的某次提交，注意该撤销操会作为一个新节点存在于分支上：

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-30.png)   
上图中显示了当前的历史提交，当我要撤销 ab1591eb4e06c1e93fdd50126b9fab8a88d89155 这个节点时，执行如下命令：   
命令：`git revert ab1591eb4e06c1e93fdd50126b9fab8a88d89155`

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-31.png)   
从图中可以看出，完成操作后，会产生一个新的节点，该节点就是撤销操作产生的。

**注意与 `git reset` 的区别**

<a name="4.2"></a>   
## 4.2. git tag: 标签的操作
用于给某次提交打个标签，例如截止到某次提交后完成了某个重大版本的开发，则可以在该次提交打上一个版本的 tag 。

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-32.png)   
例如，截止到图中的最近一次提交，我们完成了 1.0 版本的开发，则可以通过以下命令为其打上版本的标签。   
命令：`git tag v1.0` 为当前提交打上 v1.0 的标签   
命令：`git tag v1.0 ab1591eb4e06c1e93fdd50126b9fab8a88d89155` 为这个节点打上 v1.0 的标签

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-33.png)   
图中可以看出 v1.0 标签已经打上了。   
如果发现标签打错了，想删除某个标签，则可以通过如下命令来执行。   
命令：`git tag -d v1.0` 删除 v1.0 标签   

如果想将标题推送到远程库，则可以使用如下命令来完成。   
命令：`git push origin --tags` 将打的 tag 都推送到远程库

<a name="4.3"></a>   
## 4.3. git show: 显示信息
可用于显示某次提交或者某个 tag 相关的信息。   
命令：`git show commit_id` 显示某次提交的详细信息

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-34.png)

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-35.png)

命令：`git show tag_name` 显示某个 tag 的详细信息

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-36.png)

<a name="4.4"></a>   
## 4.4. git blame: 查看文件每行的提交历史（追责）
可用于查看某个文件中的每一行是那次提交产生的，是谁提交的，什么时候提交的，提交的版本号是多少等等详细信息，在实际工作中方便对出问题的代码进行追责，找到产生 BUG 的责任人。

命令：`git blame file_name`

![](https://raw.githubusercontent.com/goto456/markdown-pictures/master/wengeblog/git/git-37.png)   
上图中可以看到 `README.md` 这个文件有 5 行，其中后 4 行都是我在 2018 年提交的，第 1 行是另外一个人在 2017 年提交的。

<a name="5"></a>   
# 5. 团队协作应用
在团队协作过程中一般会有多个分支，比如有默认的 master 分支，有用于开发的 dev 分支，还有用于测试的 test 分支，用于对外发布的 release 分支，以及每个开发人员开发不同功能时用到的 feature_xx 分支等等。

公司中一般是用 GitLab 搭建的代码托管服务，几个人的小团队也可以自己搭建。

每个团队业务不一样，分支数量的设置也会不一样，下面我介绍一下我们团队的分支设置，以及普通开发人员和项目 leader 对不同分支的不同权限以及不同的操作。

<a name="5.1"></a>   
## 5.1 分支设置
我们常用的分支有3个（master 分支、dev 分支、test 分支）以及若干个 feature_xx 分支。

1. `master` 分支：是主分支，是最终上线代码的分支，该分支被设置被保护分支（锁住），普通开发人员没有权限操作，只有团队 leader 有合并的权限；
2. `dev` 分支：是用于开发的分支，该分支被设置为默认 clone 的分支，也用于合并到 master 之前进行测试的分支，普通开发人员从远程 clone 到本地的默认分支，可以进行合并等操作；
3. `test` 分支：是用于测试的分支，测试人员可以将自己开发分支中的修改合并到 test 分支在测试环境进行测试，一般该分支不合并到任何分支；
4. `feature_xx` 分支：是用户开发自己模块功能的特征分支，可以叫 feature_login, feature_ui, feature_payment 等与开发的功能相关的名称，该分支上的功能开发完、测试无误后可合并到 dev 分支上。

<a name="5.2"></a>   
## 5.2 普通开发人员的操作
普通开发人员，一般按照如下几个步骤来进行开发、测试工作就可以了：
1. 将远程 dev 分支 clone 到本地，例如：`git clone git@github.com:goto456/test.git`；
2. 从 dev 分支拉出（新建）自己的 feature 分支用于开发，例如：`git checkout -b feature_login`；
3. 在自己的 feature 分支上进行开发工作；
4. 开发完了用 add、commit 等操作提交到当前分支；
5. 如果需要在测试环境进行测试，则将远程 test 分支拉到本地，例如：`git branch test origin/test`;
6. 将自己的 feature 分支合并到 test 分支，并将 test 分支 push 到远程，例如：`git rebase test`, `git checkout test`, `git merge feature_login`, `git push origin test`；**（注意：我们推荐用 rebase 来合并，以保证分支的整洁、美观）**
7. 通过公司的发布平台将远程 test 分支发布到测试环境进行测试；
8. 如果测试没问题或者开始就不需要测试，这可以直接将当前 feature 分支合并到 dev 分支，并 push 到远程库，例如：`git rebase dev`, `git checkout dev`, `git merge feature_login`, `git push origin dev`；**（注意：我们推荐用 rebase 来合并，以保证分支的整洁、美观）**
9. 这时表示该功能已经开发完成了，代码的 review 以及发布，需要团队 leader 在合并到 master 操作时进行；这时可以删除了自己的 feature 分支，例如：`git branch -d feature_login`；
10. **如果在 push 到远程的时候提示需要先 pull 时，我们推荐使用 rebase 的方式：`git pull --rebase` 以保持分支的整洁、美观。**

<a name="5.3"></a>   
## 5.3 团队 leader 的操作
因为只有 leader 有操作 master 分支的权限，所以需要完成 dev 分支到 master 分支的合并，以及后续打 tag 和正式上线发布的工作：
1. 先切换到 dev 分支，并拉取最新的状态，例如：`git checkout dev`, `git pull --rebase origin dev`；
2. 进行代码 review 等过程后，合并到 master 分支，例如：`git rebase master`, `git checkout master`, `git merge dev`;**（注意：我们推荐用 rebase 来合并，以保证分支的整洁、美观）**
3. 为本次完成的版本打上标签，例如：`git tag v1.0 -m "release version 1.0"`；
4. 将本地合并后的 master 分支以及标签 push 到远程库，例如：`git push orgin master --tags`。

<a name="6"></a>   
# 6. 结束语
以上就是我从自己平时的应用中整理出的一个比较简洁的教程，以及我们团队在实际工作中是如何使用的。希望对大家有所帮助！

