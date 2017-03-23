#ajax

	$.ajax({
	        url: $(form).attr('action'),
	        type: $(form).attr('method'),
	        data: $(form).serialize(),//输出序列化的表单值
	        beforeSend: function () {
	            layer.load();
	        },
	        complete: function () {
	            layer.closeAll('loading');
	        },
	        error: function (XMLHttpRequest, textStatus, errorThrown) {
	            layer.alert('出错拉:' + textStatus + ' ' + errorThrown, {icon: 5});
	        },
	        success: function (data) {
	            if (!$.isFunction(func)) {
	                func = function (data) {
	                    data.url ? window.location.href = data.url : null;
	                }
	            }
	            if (data.status == 1)
	            {
	                layer.alert(data.message, {icon: 6}, function (index) {//修改之后提示信息弹框
	                    layer.close(index);
	                    func(data);
	                });
	            }
	            else {
	                layer.alert(data.message, {icon: 5}, function (index) {
	                    layer.close(index);
	                    func(data);
	                });
	            }
	        }
	    });
	    
	    
	    
	    
	    
	    
#jq赋值

给单选框赋值：

	$(":radio[name='Introduce[is_show]'][value='1']").attr("checked","checked");
	
	
	
#jquery操作select
##删除option
###清空所有option
	$("#select_id").empty();
###删除特定的option
	$("#select_id option:last").remove();
	$("#select_id option[index='0']").remove();
	$("#select_id option[value='3']").remove();
	$("#select_id option[text='4']").remove(); 
##添加option
	$("#select_id").append("<option value='Value'>Text</option>");//追加一个/option
	$("#select_id").prepend("<option value='0'>请选择</option>"); //插入一个option，第一个位置

##获取值


 

#this and $(this) 

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
