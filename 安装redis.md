##安装redis

首先是
	
	wget http://download.redis.io/releases/redis-3.2.4.tar.gz
	tar -zxvf redis-3.2.4.tar.gz
进入文件夹后make test
之后可能会提示需要暗转tcl8.5以上版本，可以使用yum安装  
**注：用yum查看可安装的版本：yum list tcl**

	yum install tcl

如果提示  

	!!! WARNING The following tests failed:
	
	*** [err]: Server is able to generate a stack trace on selected systems in tests/integration/logging.tcl
	expected stack trace not found into log file
	Cleanup: may take some time... OK
	make[1]: *** [test] Error 1
	make[1]: Leaving directory `/usr/local/src/redis-3.2.4/src'
	make: *** [test] Error 2

这个只是一个警告，可以忽略，之后
 
	make install

编译安装之后在当前目录的src子目录下会有几个文件，把他们都转移到一个文件夹中
	
	[root@VM_223_218_centos src]# cp redis-server /usr/local/redis/
	[root@VM_223_218_centos src]# cp redis-cli /usr/local/redis/
	[root@VM_223_218_centos src]# cp redis-benchmark /usr/local/redis/
	[root@VM_223_218_centos src]# cd ../
	[root@VM_223_218_centos redis-3.2.4]# ls
	00-RELEASENOTES  BUGS  CONTRIBUTING  COPYING  INSTALL  MANIFESTO  Makefile  README.md  deps  redis.conf  runtest  runtest-cluster  runtest-sentinel  sentinel.conf  src  tests  utils
	[root@VM_223_218_centos redis-3.2.4]# cp redis.conf /usr/local/redis/  

启动redis  
	
	/usr/local/redis/redis-server redis.conf//这种方式如果退出的话进程会断掉
	/usr/local/redis/redis-server &//这种方式退出了进程也会在后台运行
	ps aux | grep redis//查看redis进程情况
	chkconfig --add redis//设置开机自启动redis
	chkconfig --del redis//去除redis开机自启动


##安装php redis扩展

首先还是下载php-redis

	wget https://github.com/phpredis/phpredis/archive/develop.zip
解压  
  
	unzip develop.zip

进入解压后的目录后  

	phpize//用处待查明，如果没报错即可下一步
	whereis php-config//查出php配置文件
	./configure --with-php-config=/usr/bin/php-config//如果没有错误即可下一步
	make && make install  //在安装的过程中会提示redis.so在哪里，后面将其放置到php的扩展目录即可  
	/usr/lib/php/modules/  //这是安装是本机的php扩展目录，如果不知目录，可以在php.ini随便写一个扩展，重启php-fpm的时候就会提示扩展目录
	vim /etc/php.ini//编辑配置文件，加入redis.so
