### 添加远程库
GitHub提供给Git进行仓库托管服务，也就是一个开放的远程仓库。公有的SSH key可以公开，这样，别人也可以上传到你的仓库中，不过必须在github账号管理中添加SSH Key才可以上传。

要关联一个远程库，使用命令
`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令
`git push -u origin master`
第一次推送`master`分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

### 从远程库克隆
使用命令`git clone path`克隆到本地库。

GitHub给出的地址不止一个，还可以用`https://github.com/VillaZhang/glearngit.git`这样的地址。实际上，Git支持多种协议，默认的`git@github.com:VillaZhang/learngit.git`使用`ssh`，速度快，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放`http`端口的公司内部就无法使用`ssh`协议而只能用`https`。