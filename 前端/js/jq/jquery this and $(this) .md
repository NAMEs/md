#JQuery  

##this and $(this) 

首先是$()的含义，表示JQuery(),即返回jq对象；  

  
	alert($('#id'));
	
会弹出：[object Object ]

	$("#desktop a img").each(function(index){
	
	          alert($(this));
	
	          alert(this);
	
	}
	
这里的两个分别输出：  

	alert($(this));  弹出的结果是[object Object ]
	
	alert(this);        弹出来的是[object HTMLImageElement]
	
	
就是说：  
1. this返回的是一个html对象，是不能对其使用jq的一些函数（如val()方法），但是可以直接引用一些html的属性（如title）；  
2. $(this)返回的是一个jq对象，是可以对其使用jq的函数的，不能直接饮用一些html的属性；
