# 创建版本库
本库又名仓库，英文名repository，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。

所以，创建一个版本库非常简单，首先，选择一个合适的地方，创建一个空目录：
````
$ mkdir learngit
$ cd learngit
$ pwd
/c/Users/zyf/learngit

````
`pwd`命令用于显示当前目录。在我的PC上，这个仓库位于`/Users/michael/learngit`。

第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：
````
$ git init
Initialized empty Git repository in C:/Users/zyf/learngit/.git/
````
瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），细心的读者可以发现当前目录下多了一个`.git`的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。

如果你没有看到`.git`目录，那是因为这个目录默认是隐藏的，用`ls -ah`命令就可以看见。

第三步，编写`readme.txt`文件，内容为：
````
Git is a version control system.
Git is free software.
````
放在`learngit`文件夹下，然后告诉Git，添加到仓库，使用命令`$ git add readme.txt`(可同时提交多个文件)。
然后提交到仓库，使用命令`$ git commit -m "wrote a readme file."`。
````
$ git commit -m "wrote a readme file"
[master (root-commit) 79d1a0e] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt
````

`-m`后面输入的是本次提交的说明。至此，已经学会了如何建库，如何commit。
下次将学习版本的回退以及修改与删除。