##用yum安装mysql

	yum -y install mysql-server　← 安装MySQL
	yum -y install php-mysql　   ← 安装php-mysql

数据库的目录结构：  
1、数据库目录　/var/lib/mysql/  
2、配置文件　　/usr/share/mysql（mysql.server命令及配置文件）  
3、相关命令　　/usr/bin (mysqladmin mysqldump等命令)  
4、启动脚本　　/etc/rc.d/init.d/（启动脚本文件mysql的目录）  


###环境的配置

	chkconfig mysqld on　← 设置MySQL服务随系统启动自启动
	chkconfig --list mysqld　← 确认MySQL自启动
	/etc/rc.d/init.d/mysqld start　 ← 启动MySQL服务
	/usr/bin/mysqladmin -u root -p shutdown   //关闭mysql

###修改密码

	mysql -u root  ← 在没设置密码之时，用root用户登录MySQL服务器
	select user,host,password from mysql.user;　 ← 查看用户信息
	set password for root@localhost=password ('在这里填入root密码');　 ← 设置root密码

###用密码登陆mysql

	mysql -u root -p


###mysql设置为服务组

	/sbin/chkconfig --list   //检查系统服务组情况
	/sbin/chkconfig --add mysqld   //从系统服务组中添加mysql
	/sbin/chkconfig --del mysqld   //从系统服务组中删除mysql

###mysql常用命令行
**create database test;**//创建一个名为test的数据库  
**show databases;**//显示所有的数据库  
**select test;**//选择数据库
**show tables;**//显示数据库的表





##安装nginx
	
	yum install nginx

nginx目录结构：  
1 配置所在目录：/etc/nginx/  
2 PID目录：/var/run/nginx.pid  
3 错误日志：/var/log/nginx/error.log  
4 访问日志：/var/log/nginx/access.log  
5 默认站点目录：/usr/share/nginx/html  

开启nginx

	nginx -c /etc/nginx/nginx.conf

重启nginx  
  
	/usr/sbin/nginx -s reload


配置nginx文件

	vim etc/nginx/nginx.conf  

从里面的代码看出，还包含了其他的配置文件
找到default.conf
	
	vim etc/nginx/conf.d/default.conf  

然后配置php

	location ~ \.php$ {
        root           html;
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  /usr/share/nginx/html$fastcgi_script_name;//这里是服务器的默认站点地址
        include        fastcgi_params;
    }

防火墙开启80端口

	iptables -I INPUT -p tcp --dport 80 -j ACCEPT

##安装php

	yum install php php-fpm
	yum -y install php-gs php-xml php-mbstring php-ldap php-pear php-xmlrpc  //安装php扩展
	vim etc/php-fpm.conf  //编辑fpm配置文件  

找不到php的配置内容，然后从里面可以看出还包含了其他的配置文件

打开www.conf

	vim etc/php-fpm.d/www.conf
可以看到有个chroot的配置，这里改为

	chroot = /usr/share/nginx/html   //这里是服务器的默认站点地址

还有user和group默认是apache，这里由于是nginx，所以改成nginx用户组，这里可以在nginx的配置文件中看到

	user              nginx;

查看所有用户组可以通过这个文件查看：

	vim etc/group


  
  

把php-fpm加入为开机启动项目

	chkconfig php-fpm on
启动fmp服务

	/etc/init.d/php-fpm start

##yum安装php5.6  

配置yum源

追加CentOS 6.5的epel及remi源。

	rpm -Uvh http://ftp.iij.ad.jp/pub/linux/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
	rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

以下是CentOS 7.0的源。

	yum install epel-release
	rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

使用yum list命令查看可安装的包(Packege)。

	yum list --enablerepo=remi --enablerepo=remi-php56 | grep php
安装PHP5.6

yum源配置好了，下一步就安装PHP5.6。

	yum install --enablerepo=remi --enablerepo=remi-php56 php php-opcache php-devel php-mbstring php-mcrypt php-mysqlnd php-phpunit-PHPUnit php-pecl-xdebug php-pecl-xhprof

用PHP命令查看版本。

	php --version

##防火墙

	/etc/init.d/iptables status   //查看防火墙状态  

 

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


