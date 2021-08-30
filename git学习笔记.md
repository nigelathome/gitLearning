[toc]

##配置和初始化
###1.设置用户名和邮箱
--global对全部仓库有效，--local只对当前仓库有效，同时配置情况下local优先级高于global

```
git config --local user.name 'Hui, Li'
git config --local user.email 'nigelathome@126.com'
```
###2.查看用户名和邮箱

```
git config --local --list | grep -i user.name
git config --local --list | grep -i user.email
git config --local user.name
git config --local user.email
```
###3.已有的目录初始化为git仓库
```
cd $已有目录
git init
```
###4.新建同时初始化一个git仓库
```
git init $不存在的目录
```

##代码提交
###1.加入暂存区
将改动从工作区提交到暂存区使用git add命令。命令可以带多个文件，也可以指定一个文件

```
git add . //当前目录所有文件
git add -u //工作目录所有文件
git add file1
git add file1 file2 file3
```
###2.暂存区内容提交版本库
将提交到暂存区的改动提交到版本库

```
git commit -m '改动的内容'
```
###3.一次性将改动直接从工作区提交到版本库
```
git commit -am '改动内容'
```
###4.清除暂存区和工作目录所有改动
```
git reset --hard
```
###5.取消放入暂存区的改动
这样一个场景，已经把改动放入了暂存区但有发现工作区的改动更好，于是想把暂存区的内容撤回，通过修改工作区的改动再放入暂存区
```
git reset HEAD
git reset HEDA -- 文件1 文件2 ...
```
###6.取消放入暂存区的改动
这样一个场景，发现工作区的修改不如已经放入了暂存区的修改好，于是想把暂存区的内容同步到工作区，然后基于通过的内容继续进行开发
```
git checkout -- 文件1 文件2 ...
```
###6.删除部分的commit
找到需要回退到的commit id，例如260c93e
```
git reset --hard 260c93e
```

##分支管理
###1.查看分支
```
git branch -av 
```
###2.创建分支
```
git checkout -b 分支名 基于的历史提交id
git checkout -b 分支名 已有的分支
```
###3.切换分支
```
git checkout master
```
###4.切换commit
```
git checkout commitID
```
切换完之后处于detached HEAD（分离头指针)状态
###5.分离头指针下提交分支
```
git branch branch名字 commitID
```
###6.删除分支
```
git branch -d 分支名
```

###7.查看暂存区和head的差异
当把内容通过git add之后，更改的内容就存入了暂存区，查看暂存区和当前head的差异
```
git diff --cached
```

###8. 查看工作区和暂存区的差异
工作区就是当前没有git add的改动，比较暂存区和工作区的差异
```
git diff 
git diff -- 文件名1 文件名2 ...
```

##堆栈
###1、未提交的改动加入堆栈
```
git stash save '改动内容'
```

###2、查看堆栈记录
```
git stash list
```

###3、使用堆栈记录
通过查看堆栈记录，确定要是用的记录序号stash@{n}

```
git stash apply n
```

###4、删除某条堆栈记录
同样通过查看堆栈记录确定要删除的记录序号stash@{n}

```
git stash drop n
```

###5、删除全部堆栈记录
```
git stash clear
```

###6、使用最新的堆栈记录
```
git stash pop
```
git stash pop之后该条堆栈记录会被从堆栈中删除

##变基
###1.修改最近一次commit msg

```
git commit --amend
```

###2.修改旧的commit msg
如果是老旧的commit，通过rebase操作。先要找到需要更改的commit的`parent` commit id。例如我要修改commit是dfgga，它的parent commit id是e62cc。那么使用命令：

```
git rebase -i e62cc
```
进入交互界面。此时找到dfgga这个提交，将pick改成`reword`。然后会进入一个关于dfgga的编辑界面，进行对commit的编辑，然后保存即可

###3.合并多个连续的commit
假设这样的场景，需要合并三个连续的commit1，commit2，commit3.找到这3个commit中的父commit id。假设1是最新的commit

```
git rebase -i {父commit id}
```
进入编辑界面后，将commit2和commit3的操作变成squash（压缩）

```
pick commit1
s commit2
s commit3
pick other
```
保存后进入commit1的编辑界面，编辑commit，然后保存退出即可

###4. 合并多个不连续的commit
假设这样的创建，需要合并commit1，commit3.中间隔着一个commit2. 同样找到这2个commit中的父commit id。假设1是最新的commit

```
git rebase -i {夫commit id}
```
看到编辑界面下有这样的记录：
```
pick commit1
pick commit2
pick commit3
pick other
```
将commit3对应的删除掉，并放到commit1和commit2中间添加一条记录squash commit3，编辑记录变成：
```
pick commit1
s commit3
pick commit2
pick other
```
保存后进入commit1的编辑界面，编辑commit，然后保存退出即可

###5.合并远端分支
作用等同于git pull（git fetch然后再merge），不同的是这个操作完成之后不会产生无效的commit

```
git pull --rebase
```
###4.继续进行变基

```
git rebase --continue
```
###5.放弃编辑

```
git rebase --abort
```

##同步远程仓库 
###1.添加本地仓库到远程仓库
需要先在gitHub创建仓库，并拿到ssh地址。例如：git@github.com:nigelathome/gitLearning.git
本地仓库下执行：

```
git remote add {自定义远程仓库名} git@github.com:nigelathome/gitLearning.git
```
###2.查看远程仓库
```
git remote -v
```
###3.本地提交推往远端仓库
```
git push {远程仓库名} --all
```

##查看版本历史
###1、简洁版
只显示部分提交id值和提交信息，和当前版本信息，一目了然

```
git log --oneline
```

###2、只看最近的n次提交
使用-n次数的方式，可以组合使用--oneline。例如查看最近3次的记录

```
git log -n3 --oneline
```

###3、查看不同分支的提交
git log默认查看当前分支的提交历史。查看全部分支的提交历史`--all`。
还可以指定--oneline简洁查看历史，或者-n查看进行组合。以图形方式展示提交历史--graph

```
git log --all --graph
```

###4、查看不同提交的差异
比较commit-id分别是c8cb16c8fac96和8da976aaa
也可以使用分支名字，因为分支名本质就是对应分支的head指针，也就是最新的提交
```
git diff c8cb16c8fac96 8da976aaa
git diff 分支1  分支2
git diff 分支1  分支2  文件
```

##其他操作
###1.重命名文件
```
git mv file_old file_new
```
###2.删除文件
```
git rm file
```
###3.不需要git管理
编辑`.gitignore`文件。例如要忽略xcodeproj和xcworkspace，以及Pods下的文件
其中对一个文件名加斜杠和不加斜杠区别是：不加斜杠的表示这个文件或者这个目录下的全部文件都会被忽略；加斜杠的表示这个目录下的全部文件会被忽略，但是这个文件会被git管理。例如*.xcodeproj/代表了*.xcodeproj这个工程命令下的文件不需要git管控，而文件*.xcodeproj本身的改动是git需要管控的
```
.xcbkptlist
*.xcuserstate
xcuserdata
UserInterfaceState.xcuserstate
*.xcworkspace/
*.xcodeproj/
xcuserdata/
UserInterface.xcuserstate

Pods/
```

如果编辑之后不生效需要清理git缓存
```
git rm --cache music.xcodeproj/ -r
```

###4.查看当前状态
```
git status 
```

##说明
1.author和committer不同的场景：A分支上有个提交c1，提交人是X，我在B分支上`cherry-pick` A分支上的c1提交。然后在B上提交这个c1，此时B上的提交是c2，提交人是我，而作者是X    
2.`.git/config`仓库的配置信息；`.git/HEAD`指向的分支；`.git/refs/heads/master`存放了提交id；  
3.git对象包括commit、tree、blob、tag等。通过命令`git cat-file -t 对象id`查看；`git cat-file -p 对象id`可以查看对象内容   
4.所有的对象存储在`.git/objects`目录下，对象id由`目录前缀+目录下的哈希值构成`。例如objects下的8e目录下有个xyz，此时对象id是8exyz  
5.暂存区和版本历史的内容都会创建对应的对象，工作区的修改不会创建对象  
6.tag是里程碑的含义，可以用来给上线版本打标记  
7.头指针HEAD通常会跟一个分支绑定，例如HEAD -> master。如果HEAD不指向具体的分支，只指向commit，此时就是分离头指针的状态。分离头指针下做的commit，在切换到别的分支的时候可能会被丢弃。要想保留变更需要建立一个分支和这个变更挂钩，例如 git branch <branch-name> commit-id（基于commit-id建立分支)
8.HEAD指代当前的提交，`HEAD~n`指代当前提交的n级祖先
9. 变更暂存区的命令与git reset修改，变更工作区的命令与git checkout修改