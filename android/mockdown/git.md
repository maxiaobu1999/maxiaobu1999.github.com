##[GIT使用说明](http://www.jianshu.com/p/e32a8e7ca93b)
*  [GIT下载地址](https://git-scm.com/downloads)
    * 配置全局的name和email，这里是的你github或者bitbucket的name和email

            git config --global user.name "xkwg"
            git config --global user.email "xkwg@163.com"
*   [SSH配置](http://m.jb51.net/softjc/574997.html)
    * 在C:\Users\maxiaobu 下打开git bash
    * ssh-keygen -t rsa -C "maqinglong1999@163.com"  之后一路回车
    * 会生成.ssh文件夹

        <div align = center>

        ![.ssh文件夹](http://files.jb51.net/file_images/article/201709/2017090317190573.png)
        </div>
    * 进入.ssh文件夹，用记事本打开id_rsa.pub，复制里面的内容添加到你github或者bitbucket ssh设置里即可

        <div align = center>

        ![.ssh文件夹](http://files.jb51.net/file_images/article/201709/2017090317190575.png)
        </div>

    * 测试是否添加成功

            ssh git@github.com

      提示：“Hi lsyz0021! You've successfully authenticated, but GitHub does not provide shel l access.”说明添加成功。

* [GIT命令](https://gist.github.com/guweigang/9848271)
    * git status 可以让我们时刻掌握仓库当前的状态，下面的命令告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

            $ git status
            # On branch master  在master分支
            # Changes not staged for commit:  改变没有被提交
            #   (use "git add <file>..." to update what will be committed)
            #   (use "git checkout -- <file>..." to discard changes in working directory)
            #
            #    modified:   readme.txt
            #
            no changes added to commit (use "git add" and/or "git commit -a")
    * git diff顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到，我们在第一行添加了一个“distributed”单词。
        
            $ git diff readme.txt 
            diff --git a/readme.txt b/readme.txt
            index 46d49bf..9247db6 100644
            --- a/readme.txt
            +++ b/readme.txt
            @@ -1,2 +1,2 @@
            -Git is a version control system.
            +Git is a distributed version control system.
             Git is free software.
    * git log命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是append GPL，上一次是add distributed，最早的一次是wrote a readme file。
      如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：
      
            $ git log --pretty=oneline
            3628164fb26d48395383f8f31179f24e0882e1e0 append GPL
            ea34578d5496d7dd233c827ed32a8cd576c5ee85 add distributed
            cb926e7ea50ad11b8f9e909c05226233bf755030 wrote a readme file
    * git checkout -- file可以丢弃工作区的修改
    
            git checkout -- readme.txt
            git branch    查看当前分支：
    * 关联一个远程库
            
            it remote add origin git@server-name:path/repo-name.git
    * 创建dev分支，然后切换到dev分支
    
            $ git checkout -b dev
    * 删除文件
    
            rm test.txt
            git commit -m "remove test.txt"
    * 版本回退
        * git reset --hard HEAD^   回退上个版本
        * git reset --hard HEAD^^   回退上上个版本 
        * git reset --hard 3628164     回退至3628164版本
    * 拉代码

            git clone
            git clone git@github.com:maxiaobu1999/MVP-master.git
    * 提交代码

            git add
            git commit -m '提交的信息'
            git push
    * vim如何保存 Shift + ：，再输入q!或wq!（不保存改动，wq!是保存文件的写入修改）退出

