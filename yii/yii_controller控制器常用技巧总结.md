#yii控制器常用技巧总结
##跳转到另外一个页面（controller中）
###使用actionShow
	$control=Yii::app()->runController('site/show/id/2');  
	$control=Yii::app()->runController('site/show');  
###使用redirect  
	$this->redirect(array('/site/contact','id'=>12));  
	$this->redirect(array('site/contact','id'=>'idv','name'=>'namev'));  
	$this->redirect(array('site/contact','v1','v2','v3'));  
	$this->redirect(array('site/contact','v1','v2','v3','#'=>'ttt'));  
	$this->redirect('http://www.baidu.com');
	
##路由问题
如果一个action是actionTestRound()的话，那么它的路由就是r=控制器名/test-round