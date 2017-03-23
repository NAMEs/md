#Mac常用的命令总结

##设置别名
alias 别名 ＝ '指令名称'

##终端快捷操作
control+a:跳到终端的一开始  
control+e:跳到终端的最后  


##合并文件夹的话需要使用命令行

	cp -R d1/ d2/

其中，d1和d2表示要复制的文件夹和目标文件夹；



##brew
brew一般的安装目录在/usr/local/Cellar/下；  
###安装和卸载
brew install git  
brew uninstall git  
###查询  
brew search git  
###其他
**brew list**           列出已安装的软件  
**brew update**     更新brew  
**brew home**       用浏览器打开brew的官方网站  
**brew info**         显示软件信息  
**brew deps**        显示包依赖


##mac重置mysql的root密码

* 首先是停止mysql服务，一般是在系统偏好设置－mysql；
* 然后打开一个终端，输入：sudo /usr/local/mysql/bin/mysqld_safe --skip-grant-tables；
* 然后打开另外一个终端，输入：sudo /usr/local/mysql/bin/mysql -u root  
UPDATE mysql.user SET authentication_string=PASSWORD('新密码') WHERE User='root';  
FLUSH PRIVILEGES;  
exit;  

 