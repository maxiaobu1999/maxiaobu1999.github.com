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
    * 拉代码

            git clone
            git clone git@github.com:maxiaobu1999/MVP-master.git
    * 提交代码

            git add
            git commit -m '提交的信息'
            git push
    * vim如何保存 Shift + ：，再输入q!或wq!（不保存改动，wq!是保存文件的写入修改）退出

