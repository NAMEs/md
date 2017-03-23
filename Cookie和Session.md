##Cookie和Session

Cookie:客户端  
Session:服务器端  

##解决Cookie跨域的问题：  

- 现在的问题是有两个域名：www.a.com和www.b.com，b.com需要获取a.com的cookie；
- 解决方法如下：  

在a.com编写一个设置cookie的页面：set_cookie.php

	<script src="http://www.b.com/set_cookie.php?name=yhp"></script>//js跨域
	
在b.com编写一个设置cookie的页面：set_cookie.php   

	header("P3P: CP=CURa ADMa DEVa PSAo PSDo OUR BUS UNI PUR INT DEM STA PRE COM NAV OTC NOI DSP COR");   
	setcookie("cookie_name", $_GET['name'],time()+365*24*60*60);

在b.com编写一个获取cookie的页面：get_cookie.php  

	var_dump($_COOKIE);
	
这是因为通过script标签可以跨域传输访问一些文件，所以我们可以通过这一点实现cookie的跨域共享；