#Git的使用
*Git：分布式版本控制系统-->跟踪文本文件的改动*

##Git的安装
###Linux
先用`git`命令查看是否已安装，若无则使用

    $ sudo apt-get install git

###Mac OS X
两种方法：

  1.安装homebrew，然后通过homebrew安装Git
  
  2.直接从AppStore安装Xcode
  
##Git的基础
###创建版本仓库
*版本仓库即为仓库（repository），可简单理解成一个目录，该目录下所有文件都被Git管理起来*

    $ mkdir repository_name
    $ cd repository_name
    $ git init

`git init`命令把该目录变成Git可以管理的仓库，此时为一个空仓库（目录下有.git的目录，不要随意更改）。
###添加文件至版本库
*（该待被添加文件一定存放于repository_name下（或其子目录））*

    $ git add file_name
    $ git commit -m "description"   
    
`-m`后面输入的是本次提交的说明。可多次`add`文件（这些文件会被放入暂存区），然后一次全部`commit`
###撤销修改
`git status`可以查看当前状态，因而可以常用该命令了解仓库状态。我们可以用`git diff`查看修改的文件内容（未commit）。`git log`命令显示从最近到最远的提交日志，若使用参数`--pretty==oneline`则只显示注释内容。`git reflog`命令为查找命令记录，即调取历史命令。

`git reset --hard HEAD `版本回退。`HEAD`表示当前版本，`^`表示回退至上一个版本，则`^^`就可表示上上个版本，但若回退版本书较大，可用`HEAD~num`表示回退至之前num个版本。若在知晓版本号的情况下，则可使用`git reset --hard commit_id`直接将当前版本换为指定的版本。

就撤销操作而言，以下两个命令都是撤销：

    $ git checkout -- file
    $ git reset HEAD file
    
但是却有不同。使用`git checkout`可分为两种情况，若未使用`git add`命令，则该撤销操作将版本退为修改前原版本；反之，已添加至暂存区后又进行了修改，则该撤销操作将版本退为添加至暂存区的版本；也可将其粗略认为撤销工作区的修改。使用`git reset`命令则将撤销放在暂存区的修改，回到工作区是版本。
###删除文件
`rm file`表示将版本库的文件删除，此时工作区与版本库将不一致，有两个选择：1.使用`git rm file`命令并用`git commit`提交，确定删除此文件；2.误删了该文件，可以使用上文提及的`git checkout`命令撤销。若是真想删除该文件，可直接使用`git rm file`命令后提交。
###远程仓库
前期准备：

1.创建SSH Key（先于用户主目录下是否有.ssh文件夹且下有.id_rsa和.id_rsa.pub俩文件，若有则不必进行此步）

    $ shh-keygen -t rsa -C "youremail@example.com"    

使用该命令生成密钥，一直按回车则以默认生成。.id_rsa.pub为公钥。


2.注册Github或者bitBucket账号，将公钥复制到账号的ssh key下。

*这种做法很不方便，通过HTTP授权的方式即在clone的时候直接输入账号密码就行了*

添加远程库：

1.在Github上创建一个与本地同名的repository，后使用命令

    $ git remote add origin git@github.com:your_name/repository_name.git
    
将此远程库与本地仓库关联。`origin`是默认的远程仓库名，也可进行更改。

2.把本地库的内容推送到远程库

    $ git push -u origin master
    
用`git push`命令实际上是把当前分支`master`推送到远程。由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

    $ git push origin master    
    
从远程库克隆：

    $ git clone git@github.com:your_name/repository_name.git
    
###分支管理
创建与合并：

1.创建：

    $ git checkout -b branch_name
    
`-b`表示创建并切换分支，相当于`git branch branch_name`创建分支和`git checkout branch_name`切换分支两条命令。可用`git branch`查看分支，前面有`*`表示当前分支。完成工作之后便可提交，用`git checkout master`切换回`master`分支。

2.合并：

    $ git merge branch_name
    
`git merge`用于合并指定分支到当前分支。可以用`git log --graph --pretty=oneline --abbrev-commit`查看分支的情况。合并合并之后便可用`git branch -d branch_name`删除分支了。

*当无法自动合并分支时，需先解决冲突（Git会用`<<<<<<` `======` `>>>>>>`来标记出不同分支的内容），在提交合并。*

3.分支策略
Git用Fast forward模式时，删除分支后，会丢到分支信息。要强制禁用Fast forward模式，可在合并分支时用参数`--no-ff`。

    $ git merge --no-ff -m "description" branch_name
    
此时再用`git log`查看分支历史就会把描述写进去。

4.Bug分支

    $ git stash
    
`git stash`能把当前工作存起，等以后恢复继续工作。然后确定分支来修复bug，创建临时分支修复。`git stash list`命令可以查看所存的工作现场。要恢复有两个办法：1.用`git stash apply`恢复，但恢复后，stash内容不删除，需要用命令`git stash drop`来删除；2.用`git stash pop`恢复，此命令恢复的同时会将stash内容删除。

*`stash`可多次使用，恢复时可指定stash，使用命令`git stash apply stash_id`*

5.分支强制删除

    $ git branch -D branch_name
    
6.多人协作

查看远程库信息可用`git remote [-v]`，`-v`参数表示显示更详细的信息。可以用`git push origin branch_name`向远程库推送指定分支。

若要在自定义分支上开发，就必须创建远程的自定义分支`git checkout -b b1 origin/b1`，这就可以是不是把该分支`push`到远程。若有人向`origin/b1`推送了提交，你也对同文件做了修改，将推送失败，此时可`git pull`抓取最新的提交，在本地解决冲突合并提交。若连`git pull`也失败，就需指定本地自定义分支与远程该分支的链接。

    $ git branch --set-upstream b1 origin/b1
   
设置了链接后再`pull`。
