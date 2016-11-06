
首先修改readme.txt的内容：
````
Git is a distributed version control system.
Git is free software.
````
通过运行`$ git status`得知当前仓库状态，
````
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#    modified:   readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")
````
当前状态为：内容已经被修改，但是还没被提交。

同时使用命令`$ git diff`查看具体修改的内容，
````
$ git diff readme.txt
diff --git a/readme.txt b/readme.txt
index d8036c1..013b5bc 100644
--- a/readme.txt
+++ b/readme.txt
@@ -1,2 +1,2 @@
-Git is a version control system.
+Git is a distributed version control system.
 Git is free software.
\ No newline at end of file
````
知道了对readme.txt作了什么修改后，再把它提交到仓库就放心多了，提交修改和提交新文件是一样的两步，第一步是`git add`：
````
$ git add readme.txt
````
同样没有任何输出。在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：
```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
````
`git status`告诉我们，将要被提交的修改包括readme.txt，下一步，就可以放心地提交了：
````
$ git commit -m "add distributed"
[master 3b96b06 ] add distributed
 1 file changed, 1 insertion(+), 1 deletion(-)
 ````
提交后，我们再用`git status`命令看看仓库的当前状态：
````
$ git status
# On branch master
nothing to commit (working directory clean)
````
Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。

### 版本回退
再次修改readme.txt文件如下：
````
Git is a distributed version control system.
Git is free software distributed under the GPL.
````
然后尝试提交：
````
$ git add readme.txt
$ git commit -m "append GPL"
[master 6d44f3d] append GPL
 1 file changed, 1 insertion(+), 1 deletion(-)
 ````
已经更新了三个版本，可以使用命令`$ git log`查看：
````
zyf@Lenovo-PC MINGW64 ~/learngit (master)
$ git log
commit 6d44f3d44d4b8fe9359bc2d9df25cbc796f813f5
Author: Vincent Zhang <weixin_zh@163.com>
Date:   Sun Nov 6 09:11:08 2016 +0800

    append GPL

commit 3b96b06ed436ecaeea272503c8971f831b9dfd7d
Author: Vincent Zhang <weixin_zh@163.com>
Date:   Sun Nov 6 09:08:39 2016 +0800

    add distributed

commit 79d1a0e2fe6836925ac2e1576700417e28de37da
Author: Vincent Zhang <weixin_zh@163.com>
Date:   Sun Nov 6 09:04:38 2016 +0800

    wrote a readme file
````
然后使用`$ git log --oneline`和`$ git log --pretty=oneline`都可以进行简化：
````
zyf@Lenovo-PC MINGW64 ~/learngit (master)
$ git log --oneline
6d44f3d append GPL
3b96b06 add distributed
79d1a0e wrote a readme file

zyf@Lenovo-PC MINGW64 ~/learngit (master)
$ git log --pretty=oneline
6d44f3d44d4b8fe9359bc2d9df25cbc796f813f5 append GPL
3b96b06ed436ecaeea272503c8971f831b9dfd7d add distributed
79d1a0e2fe6836925ac2e1576700417e28de37da wrote a readme file
````
其中前面的是`commit id`（版本号）。

如果想回退到以前的版本，首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用`git reset`命令：
````
$ git reset --hard HEAD^
HEAD is now at 3b96b06 add distributed
`````
现在查看readme.txt的内容也就是是版本`add distributed`：
````
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
````
此时使用命令`$ git log`查看版本库的状态就是：
````
zyf@Lenovo-PC MINGW64 ~/learngit (master)
$ git log
commit 3b96b06ed436ecaeea272503c8971f831b9dfd7d
Author: Vincent Zhang <weixin_zh@163.com>
Date:   Sun Nov 6 09:08:39 2016 +0800

    add distributed

commit 79d1a0e2fe6836925ac2e1576700417e28de37da
Author: Vincent Zhang <weixin_zh@163.com>
Date:   Sun Nov 6 09:04:38 2016 +0800

    wrote a readme file
````
版本`append GPL`已经消失了，当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的`commit id`。Git提供了一个命令`git reflog`用来记录你的每一次命令：
````
$ git reflog
6d44f3d HEAD@{0}: reset: moving to 6d44f3
3b96b06 HEAD@{1}: reset: moving to HEAD^
6d44f3d HEAD@{2}: commit: append GPL
3b96b06 HEAD@{3}: commit: add distributed
79d1a0e HEAD@{4}: commit (initial): wrote a readme file

````
使用命令`$ git reset --hard 6d44f3d`可以回到任意一个版本,只要版本号正确就可以：
````
$ git reset --hard 6d44f3d
HEAD is now at 6d44f3d append GPL
````
版本回退原理如下：
因为Git在内部有个指向当前版本的HEAD指针，当你回退版本的时候，Git仅仅是把`HEAD`从指向`append GPL`：

![image](http://www.liaoxuefeng.com/files/attachments/001384907584977fc9d4b96c99f4b5f8e448fbd8589d0b2000/0)

改为指向`add distributed`：

![git-head-move](http://www.liaoxuefeng.com/files/attachments/001384907594057a873c79f14184b45a1a66b1509f90b7a000/0)

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪。

### 工作区和暂存区的概念

工作区即自己创建的目录，比如`learngit`文件夹便是一个工作区，而工作区里面的`.git`是一个版本库，版本库中最重要的就是被称为stage的暂存区，华友第一个分支`master`以及指向`master`的指针`HEAD`。

我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到暂存区；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。流程如下：

![image](http://www.liaoxuefeng.com/files/attachments/001384907720458e56751df1c474485b697575073c40ae9000/0)

![image](http://www.liaoxuefeng.com/files/attachments/0013849077337835a877df2d26742b88dd7f56a6ace3ecf000/0)

### 管理修改
>为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

也就是说每次修改后，如果不add到暂存区，那就不会加入到commit中。

### 撤销修改
若错误修改后工作区文件没有放到暂存区，则直接使用命令`git checkout -- file`可以放弃工作区的所有修改；但是若错误修改后的工作区文件已经被add到暂存区，则首先需要使用命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区，然后再次使用命令`git checkout -- file`便可以放弃工作区的所有修改。

### 删除文件
直接使用命令`$ git rm file`替换掉文件名即可以完成文件删除，若确定删除，则需要提交到版本库中，即`$ git commit`确定删除；若是错误删除，则需要使用命令`git checkout -- file`撤销操作，其实就是是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。