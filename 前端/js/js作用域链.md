js作用域

很多语言都是使用块级作用域，而js的是函数作用域；  
函数作用域：变量在声明它们的函数体以及这个函数体嵌套的任意函数体内都是有定义的。  
代码：  

	var scope="global";  
	function t(){  
	    console.log(scope);//函数体里已经定义变量（但是在这里还没进行赋值），会覆盖全局变量，输出undefined  
	    var scope="local"  
	    console.log(scope);  
	}  
	t(); 


**注意的是，js没有用var定义的变量都是全局变量；**   



作用域链  

代码：  

	name="lwy";  
	function t(){  
	    var name="tlwy";  
	    function s(){  
	        var name="slwy";  
	        console.log(name);  //slwy
	    }  
	    function ss(){  
	        console.log(name);  //tlwy
	    }  
	    s();  
	    ss();  
	}  
	t();//slwy and tlwy
	
	
作用域链会一直以一直往外的形式，例如例子代码中的ss()，其作用域链为：ss()->t()->window；  


另一个典型的例子：  

	<html>  
	<head>  
	<script type="text/javascript">  
	function buttonInit(){  
	    for(var i=1;i<4;i++){  
	        var b=document.getElementById("button"+i);  
	        b.addEventListener("click",function(){ alert("Button"+i);},false);  
	    }  
	}  
	window.onload=buttonInit;  
	</script>  
	</head>  
	<body>  
	<button id="button1">Button1</button>  
	<button id="button2">Button2</button>  
	<button id="button3">Button3</button>  
	</body>  
	</html>  
	
结果是三个按钮的点击事件都弹出Button4；   








