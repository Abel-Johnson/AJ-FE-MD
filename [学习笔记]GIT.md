# GIT


[在GitHub多个帐号上添加SSH公钥
](https://www.webmaster.me/uncategorized/add-multiple-ssh-keys-on-github.html)
[添加公钥以后, 依然无法push](https://www.jianshu.com/p/be58fa27a704)   ---去到`cd /Users/Johnson/.ssh`    执行`ssh-add id_rsa2(公钥名)` 就可以了

> 若执行ssh-add /path/to/xxx.pem是出现这个错误:Could not open a connection to your authentication agent，则先执行如下命令即可：ssh-agent bash

1. 配置

    ```bash
    $ git config --global user.name "Your Name"
    $ git config --global user.email "email@example.com"
    ```

2. 创建仓库

    if (想建一个空的本地仓库) {
    >
    >     
    > ```bash
    // 想建一个空的本地仓库
    > $ mkdir learngit
    > $ cd learngit
    > $ pwd(作用: 显示当前目录)
    > /Users/michael/learngit
    >    
    > ```

    }

    
    ```bash
    git init
    ```
    
    
3. 本地修改后


    `git status => git diff 可以看到底是那些修改`
    
    
4. log

    如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

    `$ git log --pretty=oneline`
    
    
5. 术语

    用HEAD表示当前版本, 上一个HEAD^, 上100个HEAD~100
    
    HEAD指向当前分支, 当前分支指向提交
    
    ![](https://cdn.liaoxuefeng.com/cdn/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)
6. 回滚

    git reset --hard HEAD^(commitID也可以)
    git push origin xxx --force(同步远程仓库)
    
8. Git删除中间某次commit

    1. 首先git log查看提交记录，找到出错的前一笔提交的commit_id
    2. 用命令git rebase -i commit_id ,查找提交记录
    3. 将出错那笔提交的pick改为drop
    4. 4，Esc，:wq
    
    (revert也可以, 但是会留一条记录)
    
    
7. 放弃
    git checkout -- file可以丢弃工作区的修改：  (总之，就是让这个文件回到最近一次git commit或git add时的状态。)
    
    git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区(撤销add)。当我们用HEAD时，表示最新的版本。
    
    一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
    
    
8. 远程仓库

    你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的
    
    1. 创建SSH Key(除非在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件)： 
    `$ ssh-keygen -t rsa -C "youremail@example.com"`
    
    2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面：，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：

    
    为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。
    涉及到push 的权限问题, 公钥不在github仓库配置里就不行
    
9. 链接远程仓库(先有本地库，后有远程库的时候)

    1. 网站建仓库
    2. $ git remote add origin git@github.com:michaelliao/learngit.git

    添加后，远程库的名字就是origin
    
    `git push -u origin master`
    我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令
    
10. SSH警告
    当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：
     
    > The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
    > RSA key fingerprint is xx.xx.xx.xx.xx.
    > Are you sure you want to continue connecting (yes/no)?
    
    这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
    
    Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
    
    Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
    这个警告只会出现一次，后面的操作就不会有任何警告了。
    
    如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。
    
11. 分支管理

Git鼓励大量使用分支：
    
> 查看分支：git branch
>     
> 创建分支：git branch <name>
>     
> 切换分支：git checkout <name>
>     
> 创建+切换分支：git checkout -b <name>
>     
> 合并某分支到当前分支：git merge <name>  ![](/Users/Johnson/学习/技术Markdown/AJ-FE-MD/笔记配图/gitmerge--no-ff.png)
>     
> 删除分支：git branch -d <name>
> 

12. 冲突

$ git add readme.txt 
$ git commit -m "conflict fixed"
[master cf810e4] conflict fixed

13. 最后，删除feature1分支：

$ git branch -d feature1



14. 发现拉错分支了, 出现了冲突, 

    1. git reset --hard commitid(本地的push和merge会形成MERGE-HEAD(FETCH-HEAD), HEAD（PUSH-HEAD）这样的引用。HEAD代表本地最近成功push后形成的引用。MERGE-HEAD表示成功pull后形成的引用。)

    2. --hard会冲掉工作区顺带暂存区


15. [rebase讲的特别好](http://gitbook.liuhui998.com/4_2.html)



## git只合并某次commit


使用场景：因开发两个分支并行开发，直接合并会造成很多问题。只是想合并某次改变的commit，就可以实用git cherry-pick
通过git log可以查看当前分支的所有提交的哈希值（ID）
切到需要合并的目标分支
运行 git cherry-pick 哈希值
如果遇到error，运行 git status查看 ummerged 下红色的文件路径，用编辑器打开，修改（寻找 === 标志， 上半是旧代码，下半是新代码，自己决定取舍）
完成所有冲突文件修改后，git add 对应文件
运行 git cherry-pick --continue 即完成。
注意：这时会弹出 一段可能会很长的描述性文字，可以全部删除，改成自己要的文字，一般我是保留最上那行，跟之前的历史描述一致，方便查找。不删除也无所谓。然后保存退出即可。
这个界面是vi的编辑界面，一次删除多行的命令为 “数字X+dd” 即可删除当前光标一下的X行内容。




[Git之使用命令行合并多次提交](https://www.jianshu.com/p/e9b6f92ea07c)
