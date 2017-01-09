#php常用函数总结

##字符串函数  
**substr()**  
返回字符串的一部分；  
语法格式：substr(string,start,length)；  

**str_replace()**  
字符串替换；  
语法格式：str\_replace(find,replace,string,count)，其中find为要查找的值，replace为替换的值；  

**substr_replace()**  
字符串替换，可以指定*从第几个字符开始，长度多少*  
语法格式：substr_replace(string,replacement,start,length)

	<?php
	echo substr_replace("Hello","world",0);
	?>
	最后输出：world

**strlen()**  
字符串长度  
语法格式：int strlen ( string $string )
  
**trim()**  
去除前后空格  
语法格式：trim(string,charlist)  其中charlist是可选去除哪些  
  
**strtolower()**  
转为小写  
语法格式：strtolower(string)  
  
**strtoupper()**  
转为大写  
语法格式：strtoupper(string)  

**str_pad()**
str_pad() 函数把字符串填充为新的长度。

	str_pad(string,length,pad_string,pad_type)  

第四个参数：  

	STR_PAD_BOTH - 填充字符串的两侧。如果不是偶数，则右侧获得额外的填充。  
	STR_PAD_LEFT - 填充字符串的左侧。  
	STR_PAD_RIGHT - 填充字符串的右侧。默认。  

##进制函数

base_convert()
进制转换函数，任意进制，返回一个字符串

	base_convert(number,frombase,tobase)
	

**bindec()**   
bindec() 函数把二进制转换为十进制。

	bindec(binary_string)//binary_string必需。规定要转换的二进制数。 
	

  
##数据库函数  
**mysql\_connect**  
建立数据库连接  
语法格式：mysql\_connect(server,user,pwd,newlink,clientflag)  
其中：  
server为服务器，user为用户名，pwd为密码；  
newlink为如果用同样的参数第二次调用 mysql\_connect()，将不会建立新连接，而将返回已经打开的连接标识。参数 new\_link 改变此行为并使 mysql\_connect() 总是打开新的连接，甚至当 mysql\_connect() 曾在前面被用同样的参数调用过。  
clientflag为client\_flags 参数可以是以下常量的组合：  
MYSQL\_CLIENT\_SSL - 使用 SSL 加密  
MYSQL\_CLIENT\_COMPRESS - 使用压缩协议  
MYSQL\_CLIENT\_IGNORE_SPACE - 允许函数名后的间隔  
MYSQL\_CLIENT\_INTERACTIVE - 允许关闭连接之前的交互超时非活动时间   
  
mysql_close  
关闭数据库连接  
语法格式：mysql\_close();  
  
mysql\_select\_db  
选择数据库  
语法格式：mysql\_select\_db(database,connection)  
  
mysql\_query  
查询mysql  


mysql\_affected\_rows  
查询上一次影响到的行数
  
  
mysql\_insert\_id()  
获取上一次插入操作产生的id
 
 
 