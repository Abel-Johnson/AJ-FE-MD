> 检测什么被修改了而且没提交
> `$ git status`
  (use "git pull" to update your local branch)  
> `$ git branch`查看本地所有分支
> 
> 
> **上线前**
> 
> 1. 先切到本地dev`$ git checkout dev`,
> 2. 然后拉取最新代码(bitbucket的dev)`$ git pull origin dev`下载代码到本地,快速合并


---
# 接到一个新任务!!!

1. 切到dev分支,然后添加分支,并进入该分支
	`$ git checkout dev`
	`$ git checkout -b add-album-tag`
	
	> `$ git branch`
	> * add-album-tag
	>   album-double-search
	>   dev
	> ...
  

2. 把修改添加到暂存区
	`$ git add .`

3. 提交到本地的当前分支
	`$ git commit -m "xxx"`
  
4. push到线上对应分支
	`$ git push origin add-album-tag`

	>  添加密钥
	> `$ cat ~/.ssh/id_rsa.pub | pbcopy`

-------

# 做出修改后,想阶段性提交!!!

 添加到提交队列,然后提交,然后同步线上分支:
 
1. `$ git add . `
1. `$ git commit`
1. `$ git push origin add-album-tag `

-----


# 当前任务完成后,把自己分出来的分支merge到dev!!!!


1. `$ git checkout dev`切换到dev分支
2. `$ git merge add-album-tag`在本地合并指定分支到当前分支
---[dev c031eed] Merge branch 'add-album-tag' into dev

	> 	#### 发生冲突了,手动解决
	> 	> *CONFLICT (content): Merge conflict in src/js/components/
	> 	> Automatic merge failed; fix conflicts and then commit the result.*
	> 	
	> 	`$ git diff`
	> 	
	> 	手动修改后
	> 	
	> 	`$ git add .`
	> 	`$ git commit -m`
	
3. `$ git push origin dev`最后push到云端对应dev分支

***最好不要自己merge,先pull request,让同事review一下,他同意(approve)以后,再merge***


---
常见问题
# git add, git commit 添加错文件 撤销

1. Git add 添加 多余文件 
这样的错误是由于， 有的时候 可能

	git add . （空格+ 点） 表示当前目录所有文件，不小心就会提交其他文件
	
	git add 如果添加了错误的文件的话
	
	**撤销操作**
	
	git status 先看一下add 中的文件 
	git reset HEAD 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了 
	git reset HEAD XXX/XXX/XXX.Java 就是对某个文件进行撤销了

2. git commit 错误

	如果不小心 弄错了 git add后 ， 又 git commit 了。 
	先使用 
	git log 查看节点 
	commit xxxxxxxxxxxxxxxxxxxxxxxxxx 
	Merge: 
	Author: 
	Date:
	
	然后 
	git reset commit_id
	
	over
	
	PS：还没有 push 也就是 repo upload 的时候
	
	git reset commit_id （回退到上一个 提交的节点 代码还是原来你修改的） 
	git reset –hard commit_id （回退到上一个commit节点， 代码也发生了改变，变成上一次的）

3. 如果要是 提交了以后，可以使用 git revert

	还原已经提交的修改 
	此次操作之前和之后的commit和history都会保留，并且把这次撤销作为一次最新的提交 
	git revert HEAD 撤销前一次 commit 
	git revert HEAD^ 撤销前前一次 commit 
	git revert commit-id (撤销指定的版本，撤销也会作为一次提交进行保存） 
	git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去，版本会递增，不影响之前提交的内容。
	
	
# cannot lock ref
### 错误描述
###### git pull的时候
> error: cannot lock ref 'refs/remotes/origin/dev': ref refs/remotes/origin/dev is at 19070aed6873f8d58f35e4631272b59f13927a1c but expected 8a5b3bda0778070bd6b92123556475c9484e04b8From 120.26.77.241:yourWorkspace/yourProject

### 解决办法: 
###### 当时是又执行了一遍命令好的
> 网上推荐的方法:
> rm .git/refs/remotes/origin/dev
> 
> git fetch


# 批量删除branch中新加的文件(untracked files)

首先确认要删除的文件
git clean -fd -n

如果以上命令给出的文件列表是你想删除的， 那么接下来执行

git clean -f -d或者git clean -fd就可以了。

其中-f表示文件-d表示目录, 如果还要删除.gitignore中的文件那么再加上-x (-x对我来说没用）

如果git submodule中也存在需要删除的文件那么需要再加个-f， 变成git clean -dff




# 分支重命名
1. 本地分支重命名

Git branch -m oldbranchname newbranchname


2. 远程分支重命名 (假设本地分支和远程对应分支名称相同)

a. 重命名远程分支对应的本地分支

git branch -m old-local-branch-name new-local-branch-name

b. 删除远程分支

git push origin  :old-local-branch-name

c. 上传新命名的本地分支

git push origin  new-local-branch-name: new-local-branch-name