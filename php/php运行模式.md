##CGI  

CGI即通用网关接口(Common Gateway Interface)，它是一段程序，通俗的讲CGI就象是一座桥，把网页和WEB服务器中的执行程序连接起来，它把HTML接收的指令传递给服务器的执行程序，再把服务器执行程序的结果返还给HTML页。CGI 的跨平台性能极佳，几乎可以在任何操作系统上实现。

CGI方式在遇到连接请求（用户 请求）先要创建cgi的子进程，激活一个CGI进程，然后处理请求，处理完后结束这个子进程。这就是fork-and-execute模式。所以用cgi方式的服务器有多少连接请求就会有多少cgi子进程，子进程反复加载是cgi性能低下的主要原因。都会当用户请求数量非常多时，会大量挤占系统的资源如内 存，CPU时间等，造成效能低下。  


##Fast-CGI

fast-cgi 是cgi的升级版本，FastCGI像是一个常驻(long-live)型的CGI，它可以一直执行着，只要激活后，不会每次都要花费时间去fork一 次。PHP使用PHP-FPM(FastCGI Process Manager)，全称PHP FastCGI进程管理器进行管理。

###FastCGI的工作原理
1. Web Server启动时载入FastCGI进程管理器(IIS ISAPI或Apache Module)  
2. FastCGI进程管理器自身初始化，启动多个CGI解释器进程(可见多个php-cgi)并等待来自Web Server的连接  
3. 当客户端请求到达Web Server时，FastCGI进程管理器选择并连接到一个CGI解释器。Web server将CGI环境变量和标准输入发送到FastCGI子进程php-cgi。  
4. FastCGI子进程完成处理后将标准输出和错误信息从同一连接返回Web Server。当FastCGI子进程关闭连接时，请求便告处理完成。**FastCGI子进程接着等待并处理来自FastCGI进程管理器(运行在Web Server中)的下一个连接。** 在CGI模式中，php-cgi在此便退出了。  

在上述情况中，你可以想象CGI通常有多慢。每一个Web请求PHP都必须重新解析php.ini、重新载入全部扩展并重初始化全部数据结构。使用FastCGI，所有这些都只在进程启动时发生一次。一个额外的 好处是，持续数据库连接(Persistent database connection)可以工作。