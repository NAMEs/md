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
 