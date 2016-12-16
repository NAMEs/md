##json 前端部分

###js便利json对象

	function (data) 
	{
		data = eval("("+data+")");//data为json字符串，这里转为json对象
		var html_content = "<option>请选择</option>";
		for (var i in data)//遍历json对象，这里用类似遍历数组的形式遍历json对象
		{
			html_content += "<option value='"+i+"'>"+data[i]+"</option>";
		}
		console.log(data);
		$("#singleitemform-id").html(html_content);
	}