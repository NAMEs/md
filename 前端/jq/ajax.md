##格式

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