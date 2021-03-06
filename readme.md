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
git rest --hard
```
##分支管理
###1.查看本地分支
```
git branch -v
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
git branch名字 commitID
```
###6.删除分支
```
git branch -d 分支名
```
##状态查看
###1.查看提交状态
```
git status 
```
###2.查看提交历史
--oneline展示简单的提交信息，-n可以查看最近n次提交
--graph图形方式展示提交历史
--all查看所有分支的历史
```
git log --oneline
git long --all 
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

##补充说明
1.author和committer不同的场景：A分支上有个提交c1，提交人是X，我在B分支上`cherry-pick` A分支上的c1提交。然后在B上提交这个c1，此时B上的提交是c2，提交人是我，而作者是X    
2.`.git/config`仓库的配置信息；`.git/HEAD`指向的分支；`.git/refs/heads/master`存放了提交id；  
3.git对象包括commit、tree、blob、tag等。通过命令`git cat-file -t 对象id`查看；`git cat-file -p 对象id`可以查看对象内容   
4.所有的对象存储在`.git/objects`目录下，对象id由`目录前缀+目录下的哈希值构成`。例如objects下的8e目录下有个xyz，此时对象id是8exyz  
5.暂存区和版本历史的内容都会创建对应的对象，工作区的修改不会创建对象  
6.tag是里程碑的含义，可以用来给上线版本打标记  
7.分离头指针下做的commit，在切换到别的分支的时候不能查看需要做新重新建分支才能看到
8.HEAD指代当前的提交，HEAD~n指代当前提交的n级祖先