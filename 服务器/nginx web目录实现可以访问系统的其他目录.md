#nginx web目录实现可以访问系统的其他目录  

nginx+php使用open_basedir限制站点目录防止跨站  
以下三种设置方法均需要PHP版本为5.3或者以上。  

方法1）在Nginx配置文件中加入  
fastcgi_param  PHP_VALUE  "open_basedir=$document_root:/tmp/:/proc/";  
通常nginx的站点配置文件里用了include fastcgi.conf;，这样的，把这行加在fastcgi.conf里就OK了。  
如果某个站点需要单独设置额外的目录，把上面的代码写在include fastcgi.conf;这行下面就OK了，会把fastcgi.conf中的设置覆盖掉。  
这种方式的设置需要重启nginx后生效。  

方法2）在php.ini中加入：  
[HOST=www.server110.com]  
open\_basedir=/home/www/www.server110.com:/tmp/:/proc/  
[PATH=/home/www/www.server110.com]  
open\_basedir=/home/www/www.server110.com:/tmp/:/proc/  
这种方式的设置需要重启php-fpm后生效。  

方法3）在网站根目录下创建.user.ini并写入：  
open\_basedir=/home/www/www.server110.com:/tmp/:/proc/  
这种方式不需要重启nginx或php-fpm服务。安全起见应当取消掉.user.ini文件的写权限。  
关于.user.ini文件的详细说明：  
http://php.net/manual/zh/configuration.file.per-user.php  

设置open\_basedir的同时最好禁止下执行命令的函数，比如：    
shell\_exec('ls /etc')仍然查看到/etc目录的文件列表  
shell\_exec('cat /etc/passwd')仍可查看到/etc/passwd文件的内容  

建议禁止的函数如下：  

	disable_functions = pcntl_alarm, pcntl_fork, pcntl_waitpid, pcntl_wait,  
	pcntl_wifexited, pcntl_wifstopped, pcntl_wifsignaled, pcntl_wexitstatus,
	pcntl_wtermsig, pcntl_wstopsig, pcntl_signal, pcntl_signal_dispatch, 
	pcntl_get_last_error, pcntl_strerror, pcntl_sigprocmask, pcntl_sigwaitinfo, 
	pcntl_sigtimedwait, pcntl_exec, pcntl_getpriority, pcntl_setpriority, eval, popen,
	passthru, exec, system, shell_exec, proc_open, proc_get_status, chroot, chgrp, 
	chown, ini_alter, ini_restore, dl, pfsockopen, openlog, syslog, readlink, symlink, 
	popepassthru, stream_socket_server, fsocket, chdir
