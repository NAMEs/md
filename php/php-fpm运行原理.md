代理和反向代理  
正向代理：访问谷歌，由于谷歌被墙，访问谷歌需要vpn翻墙才能过去，对于我们来说，vpn服务器是可以感知的，而对于谷歌服务器来说，它是感受不到vpn服务器的，这样vpn就是正向代理服务器；  
反向代理：访问百度，访问百度的时候百度会有一个代理服务器来做负载均衡，路由到不同的server，对于这个代理服务器我们是感受不到的（我们只知道访问的是百度的服务器），而对于多个百度服务器来说，这个代理服务器是可以感知的，这样的代理服务器就是反向代理服务器；  

nginx：一个高性能的HTTP和**反向代理**服务器，也是一个IMAP/POP3/SMTP服务器；  

cgi,fast-cgi协议：早起的webserver只是处理静态页面，之后出现了如php之类的动态语言，这时候就需要交给php解释器来处理了，而如何解决webserver和php解释器之间的交互呢，这时就出现了cgi协议，只要根据cgi协议去编写程序，就能解决这个问题，而fast-cgi是cgi的升级版本，原本的webserver每收到一个请求，就会fork一个cgi进程，这样太浪费资源了，而一个fast-cgi进程可以处理多个请求，大大节省了资源；
  
php-fpm：php-Fastcgi Process Manager  
进程包含 **master** 进程和 **worker** 进程两种进程，其中master 进程只有一个，负责监听端口，接收来自 Web Server 的请求，而 worker 进程则一般有多个(具体数量根据实际需要配置)，每个进程内部都嵌入了一个 PHP 解释器，是 PHP 代码真正执行的地方。   

![php-fpm进程](https://segmentfault.com/image?src=http://static.zybuluo.com/ericliu001/skg9f8pdzv9zvkp4c2by51qd/image_1b08h6i849is2hkqs1uct136h4e.png&objectId=1190000007322358&token=9e7498ecd3f8e69698c0076f4fd8af3f)


nginx和php-fpm  
nign是一个反向代理服务器，通过反向代理功能传到后端的php-fpm进行处理；  

nignx配置文件解释：  

	server {
	    listen       80; #监听80端口，接收http请求
	    server_name  www.example.com; #就是网站地址
	    root /usr/local/etc/nginx/www/huxintong_admin; # 准备存放代码工程的路径
	    #路由到网站根目录www.example.com时候的处理
	    location / {
	        index index.php; #跳转到www.example.com/index.php
	        autoindex on;
	    }   
	
	    #当请求网站下php文件的时候，反向代理到php-fpm
	    location ~ \.php$ {
	        include /usr/local/etc/nginx/fastcgi.conf; #加载nginx的fastcgi模块
	        fastcgi_intercept_errors on;
	        fastcgi_pass   127.0.0.1:9000; #nginx fastcgi进程监听的IP地址和端口
	    }
	
	}
	
	
	
流程图：  

	 www.example.com
	        |
	        |
	      Nginx
	        |
	        |
	路由到www.example.com/index.php
	        |
	        |
	加载nginx的fast-cgi模块
	        |
	        |
	fast-cgi监听127.0.0.1:9000地址
	        |
	        |
	www.example.com/index.php请求到达127.0.0.1:9000
	        |
	        |
	php-fpm 监听127.0.0.1:9000
	        |
	        |
	php-fpm 接收到请求，启用worker进程处理请求
	        |
	        |
	php-fpm 处理完请求，返回给nginx
	        |
	        |
	nginx将结果通过http返回给浏览器
