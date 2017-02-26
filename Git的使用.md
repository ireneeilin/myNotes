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

2.把本地库的东西推送到远程库

    $ git push -u origin master
    
用`git push`命令实际上是把当前分支`master`推送到远程。由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

    $ git push origin master
    
