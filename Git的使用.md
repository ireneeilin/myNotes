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
`git status`可以查看当前状态，因而可以常用该命令了解仓库状态。我们可以用`git diff`查看修改的文件内容（未commit？）。
`git log`命令显示从最近到最远的提交日志，若使用参数`--pretty==oneline`则只显示注释内容。
