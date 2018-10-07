git学习

$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit

如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。


## 步骤
1. 第一步，用命令git add告诉Git，把文件添加到仓库：  
	`$ git add readme.txt`  
	执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

2. 第二步，用命令git commit告诉Git，把文件提交到仓库：
	`$ git commit -m "wrote a readme file"`   
	简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
	 
	[master (root-commit) cb926e7] wrote a readme file
	1 file changed, 2 insertions(+) create mode 100644 readme.txt
	
> 多次add,一次commit  
> $ git add file1.txt  
> $ git add file2.txt file3.txt  
> $ git commit -m "add 3 files.  
> 
> 
> 


修改文件以后

git status上面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

git diff查看具体修改的地方

确认修改后要提交

git add .
git status
git commit -m '说明文字'
git status提交后,再查看本地仓库的提交状态,得到nothing to commit (working directory clean)Git告诉我们当前没有需要提交的修改，而且，工作目录是干净（working directory clean）的。


## 时光机
commit相当于一个存档,每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

git log查看提交历史记录


* HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

* 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
 
* 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。




git remote add origin 本地库关联远程仓库

把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程

从现在起，只要本地作了提交，就可以通过命令：

$ git push origin master

如果要推送其他分支，比如dev，就改成：

$ git push origin dev



你的小伙伴要在dev分支上开发，就必须创建远程origin的dev分支到本地，于是他用这个命令创建本地dev分支：

$ git checkout -b dev origin/dev


现在，他就可以在dev上继续修改，然后，时不时地把dev分支push到远程：
$ git commit -m "add /usr/bin/env"
[dev 291bea8] add /usr/bin/env
 1 file changed, 1 insertion(+)
$ git push origin dev





查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>