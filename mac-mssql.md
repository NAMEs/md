##mac连接mssql纪录

1.安装unixODBC
去官网直接下载，之后进行./configure, make, sudo make install，这里要注意安装路径，我这里是默认的安装路径/usr/local/，之后装freetds的时候要用到


2.安装freeTDS
照样是从官网下载，具体可见博客：  

	http://blog.csdn.net/qdujunjie/article/details/30063279   
之后要进行配置：  

	./configure --with-tdsver=8.0 --with-unixodbc=/usr/local  
从这里可以看出这里是设置了unixodbc的安装目录



安装后freeTDS好像是默认版本是5.0，这个可以通过命令行查看：  

	/usr/local/freetds/bin/tsql -C  
	
这个版本为5.0就会导致连接的时候报以下错误：  

	Error 20017 (severity 9):
		Unexpected EOF from the server
		OS error 36, "Operation now in progress"
	Error 20002 (severity 9):
		Adaptive Server connection failed
	There was a problem connecting to the server

顺便说一下连接的语句：  

	/usr/local/freetds/bin/tsql -H 192.168.137.41 -p 1433 -U sa

这时就需要修改freetds的配置文件了，去到/usr/local/freetds/etc/freetds.conf,在这个文件中修改版本信息，然后重启apache，再次连接，这时候看到：  

	locale is "zh_CN.UTF-8"
	locale charset is "UTF-8"
	using default charset "UTF-8"
	
表明已经可以连接了

前两步的安装步骤可参考博客：  

	http://blog.nguyenvq.com/blog/2010/05/16/freetds-unixodbc-rodbc-r/


3.需要给php安装扩展支持mssql和dbo\_dblib。  
进到一个php的源码包，进入ext／，然后进入mssql和dbo\_dblib这两个文件夹，分别进行配置，安装，具体操作可见以下博客：  
  
	http://blog.andyhunt.info/2013/11/29/php-mssql-pdo_dblib-freetds-support-on-mac-osx-10-9-mavericks/  
这里要注意的是，你要安装了freetds之后才能进行这一步，因为配置那里需要制定freetds的路径；

安装完之后，so文件会被放到php的扩展目录下，最后要在php.ini文件中加入语句，表明添加这两个扩展；


这时候就可以通过php程序连接mssql数据库了，测试代码如下：  

	<?php
	
	try{
	    $hostname = "192.168.137.41";
	    $port = 1433;
	    $dbname = "games";
	    $username = "sa";
	    $pw = "mj19930115";
	    $dbh = new PDO("dblib:host=$hostname:$port;dbname=$dbname","$username","$pw");
	} catch(PDOException $e)
	{
	echo "Failed to get DB handle:".$e->getMessage()."\n";
	exit;
	}
	//var_dump($dbh);
	
	?>
	
	
	
##纪录一个问题：
在使用mac链接mssql数据库的时候，用以上的配置连上了，但是编码出现问题，所有从数据库里获取的中文数据都是乱码，修改了各种都没用。后面想到可能是mac配置问题，就想到mac链接mssql是需要freetds的，所以回来看了一下freetds的配置文件，在里面修改了全局对的client charset 为utf-8，同时把tds的版本设置为7.0（版本设置这个还不清楚原因）之后，就可以了，所以得出一个结论，数据库的交互是通过freetds来的，包括之间的数据编码转换；


