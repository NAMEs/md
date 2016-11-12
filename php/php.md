#PHP Smarty使用
1、下载smarty将解压后的libs目录copy到项目目录下。

2、新建一个php文件，假如和libs目录同一级上。命名为smarty_test.php，然后增加两个目录一个为Templates文件夹，另一个为Templates_c目录，前者是以后模板文件要存放的目录，后者是smary编译后的文件存放目录。

3、在Templates目录下建立一个template.htm文件，输入以下代码：

		<html>
		<head>
		<style type="text/css">
		.bold{
		    font-weight:bold;
		    font-size:12px;
		    padding:10px;
		    width: 300px;
		    border:solid 1px blue;
		    line-height:20px;
		}
		</style>
		</head>
		<body>
		<div class="bold">{{$test}}</div>
		</body>
		</html>
4、在smart_test.php中输入以下代码：

		<?php
		include_once('./libs/Smarty.class.php');  
		//如果在php.ini文件中将include_path添加了smart的目录这里就直接写Smarty.class.php就可以了。
		
		$smarty = new Smarty();
		$smarty -> template_dir = "./Templates";     //模板存放目录
		$smarty -> compile_dir = "./Templates_c";     //编译目录
		$smarty -> left_delimiter = "{{";             //左定界符
		$smarty -> right_delimiter = "}}";             //右定界符
		$smarty -> assign('test','if success display this contents.');
		$smarty -> display('template.htm');
		?>

#php cache  
主要使用到ob函数来进行缓存，然后将缓存的内容保存到特定文件中。  
##页面缓存

		<?php
		$_time =10;
		$dir="D:php";
		function cache_start($_time, $dir)
		{
		  $cachefile = $dir.'/'.sha1($_SERVER['REQUEST_URI']).'.html';
		  $cachetime = $_time;
		  ob_start();
		  if(file_exists($cachefile) && (time()-filemtime($cachefile) < $cachetime))
		  {
		    include($cachefile);
		    ob_end_flush();
		    exit;
		  }
		}
		function cache_end($dir)
		{
		  $cachefile = $dir.'/'.sha1($_SERVER['REQUEST_URI']).'.html';
		  $fp = fopen($cachefile, 'w');
		  fwrite($fp, ob_get_contents());
		  fclose($fp);
		  ob_end_flush();
		}
		cache_start($_time, $dir);
		//以下是输出的内容，放在cache_start和cache_end两个方法之间
		for ($i=0;$i<5;$i++)
		{
		  echo $i;
		  sleep(1);
		}
		cache_end($dir);
		?>

#php获取http信息 

		/** 
		* 获取HTTP请求原文 
		* @return string 
		*/
		function get_http_raw() { 
		$raw = ''; 
		 
		// (1) 请求行 
		$raw .= $_SERVER['REQUEST_METHOD'].' '.$_SERVER['REQUEST_URI'].' '.$_SERVER['SERVER_PROTOCOL']."\r\n"; 
		 
		// (2) 请求Headers 
		foreach($_SERVER as $key => $value) { 
		if(substr($key, 0, 5) === 'HTTP_') { 
		$key = substr($key, 5); 
		$key = str_replace('_', '-', $key); 
		 
		$raw .= $key.': '.$value."\r\n"; 
		} 
		} 
		 
		// (3) 空行 
		$raw .= "\r\n"; 
		 
		// (4) 请求Body 
		$raw .= file_get_contents('php://input'); 
		 
		return $raw; 
		}