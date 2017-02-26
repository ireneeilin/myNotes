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
`git status`可以查看当前状态，因而可以常用该命令了解仓库状态。我们可以用`git diff`查看修改的文件内容（未commit？）。`git log`命令显示从最近到最远的提交日志，若使用参数`--pretty==oneline`则只显示注释内容。`git reflog`命令为查找命令记录，即调取历史命令。

`git reset --hard HEAD `版本回退。`HEAD`表示当前版本，`^`表示回退至上一个版本，则`^^`就可表示上上个版本，但若回退版本书较大，可用`HEAD~num`表示回退至之前num个版本。若在知晓版本号的情况下，则可使用`git reset --hard commit_id`直接将当前版本换为指定的版本。

就撤销操作而言，以下两个命令都是撤销：

    $ git checkout -- file
    
    
    $ git reset HEAD file
       
但是却有不同。使用`git checkout`可分为两种情况，若未使用`git add`命令，则该撤销操作将版本退为修改前原版本；反之，已添加至暂存区后又进行了修改，则该撤销操作将版本退为添加至暂存区的版本；也可将其粗略认为撤销工作区的修改。使用`git reset`命令则将撤销放在暂存区的修改，回到工作区是版本。
