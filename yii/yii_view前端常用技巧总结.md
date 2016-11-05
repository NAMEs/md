#yii前端view常用技巧总结  
##修改activeform的样式
	<?= $form->field($model,"content")->
		textInput(['maxlength' => 255,'placeholder'=>'请输入专题链接','style'=>"height:100px;"])
		->hint('Please enter your daily content here'); 
	?>
在input()中可以添加各种属性，包括样式style；

##添加js代码

	$this->view->registerJs($script, View::POS_READY);