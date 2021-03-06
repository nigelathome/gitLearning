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
git init $仓库名字
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
```
###3.切换分支
```
git checkout master
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