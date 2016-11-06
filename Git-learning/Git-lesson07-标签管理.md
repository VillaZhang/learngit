Git的标签是版本库的快照，但其实它就是指向某个commit的指针（但标签不能移动）。

### 创建标签
+ 命令`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个`commit id`；

+ `git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

+ `git tag -s <tagname> -m "blablabla..."`可以用`PGP`签名标签；

+ 命令`git tag`可以查看所有标签。
+ 
### 操作标签
+ 命令`git push origin <tagname>`可以推送一个本地标签；

+ 命令`git push origin --tags`可以推送全部未推送过的本地标签；

+ 命令`git tag -d <tagname>`可以删除一个本地标签；

+ 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。