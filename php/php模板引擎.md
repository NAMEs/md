#yii中前端数据渲染
##controller
	class Controller  
	{  
	    private $name='';  
	  
	    public function __construct($name)  
	    {     
	        $this->name = $name;  
	    }     
	  
	    public function render($viewName, $data)  
	    {     
	        extract($data, EXTR_PREFIX_SAME,'data');  
	        ob_start();  
	        ob_implicit_flush(0);  
	  
	        require($viewName . '.php');  
	        echo ob_get_clean();  
	    }     
	}  
	  
	$ctrl = new Controller('php');  
	$ctrl->render('view', array('age'=>20));  //模仿render方法调用
##view
	echo $this->name . "\n" . $age . "\n";    //view.php只有一句,其中$this是表示controller	
其中控制器中的render函数中通过这几行：  

	        extract($data, EXTR_PREFIX_SAME,'data');  
	        ob_start();  
	        ob_implicit_flush(0);  
	  
	        require($viewName . '.php');  
	        echo ob_get_clean();  

进行页面渲染；本来view.php中是没有$this和$age的，但是因为require时给予了它render方法的作用域，并且采用了extract方法操作传入的数组参数，使得读取这两个变量成为可能。




##php最简单的模板引擎实现：
###php代码
	<?php
	$a = array(
	 'a','b','c'
	);
	 
	require  'template/demo.php';//引用模板
	?> 
###模板代码
	模板文件： 
	 
	<!DOCTYPE html>
	<html lang="zh">
	<head>
	 <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
	 <title> 模板测试 </title>
	</head>
	<body>
	<?=$a[1]?>
	<pre>
	<?php print_r($a); ?>
	<pre>
	</body>
	</html>
