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