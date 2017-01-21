第一次使用git,对一些操作和概念有点模糊，在实践中慢慢得出结论，所以记录下：

#本地版本

每个人本地都有一个版本，如果想要给别人看你的版本，那么就要通过一个远程git仓库；  
  
使用gitbash在一个目录下git init，这样就创建了一个版本；  

通过ls -al可以查看本地的操作记录？ 

#远程仓库

查看远程仓库  

	$ git remote -v

通过clone命令可以复制远程仓库的代码： 
  
	$ git clone git@58.249.57.254:/home/git/interface_system

要上传到远程仓库，需要先本地上传，然后才能push上去，比如要上传到 
 
	git add README   
	git commit -m 'first commit'  
	git push origin master  

**查看版本**   

	git log --stat

**从远程更新**  

	git pull origin master(主机名 分支名)

如果本地删除了一个文件，想从git上面再拉取下来
git checkout .(这是所有)
git checkout a.php(这是单个文件)


**更新的时候遇到merge问题**

	error: Your local changes to the following files would be overwritten by merge:
	        protected/config/main.php
	Please, commit your changes or stash them before you can merge.

如果希望保留生产服务器上所做的改动,仅仅并入新配置项, 处理方法如下:

	git stash
	git pull
	git stash pop

然后可以使用git diff -w +文件名 来确认代码自动合并的情况.

反过来,如果希望用代码库中的文件完全覆盖本地工作版本. 方法如下:

	git reset --hard
	git pull
其中git reset是针对版本,如果想针对文件回退本地修改,使用

	git checkout HEAD file/to/restore