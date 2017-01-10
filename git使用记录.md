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
	
	
	
	
##gitignore

gitignore文件是用来忽略一些文件，以免提交到远程仓库，对其它人造成影响；
###常用规则
                                                                                                                         
	# 忽略掉所有文件名是 foo.txt的文件.
	foo.txt
	# 忽略所有生成的 html文件,
	*.html
	# foo.html是手工维护的，所以例外.
	!foo.html
	# 忽略所有.o和 .a文件.
	*.[oa]
	# 忽略*.b和*.B文件，my.b除外
	*.[bB]
	!my.b
	# 忽略dbg文件和dbg目录
	dbg
	# 只忽略dbg目录，不忽略dbg文件
	dbg/
	# 只忽略dbg文件，不忽略dbg目录
	dbg
	!dbg/
	# 只忽略当前目录下的dbg文件和目录，子目录的dbg不在忽略范围内
	/dbg

###追加规则无效的解决方法
一个工程的过程中如果有新的文件需要gitignore的话，在.gitignore文件添加之后发现下次commit还是无效，那是因为.gitignore对已经追踪(track)的文件是无效的，需要清除缓存，清除缓存之后文件会以未追踪的形式出现，这时候重新进行add和commit就可以了；  
清除缓存的语句：  

	// 不要忘了后面的 . 
	git rm -r --cached .
	
	
