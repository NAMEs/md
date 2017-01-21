##yii 数据库操作

##query builder
	$query = new \yii\db\Query();
	$query->select()->from()->join()->all()
	$query->select("＊")->from("Guild g")
	->leftJoin("GuildMasterApply gm",'gm.id = g.id')
	->where()

具体可以看以下博客：  
[yii2 query builder](http://blog.csdn.net/hzqghost/article/details/44117081)


##AR

使用AR类进行连表查询的例子  

	Order::find()
	->joinWith(['books b'], true, 'INNER JOIN')
	->where(['b.category' => 'Science fiction'])
	->all();
	
	
	
更多的内容可以查看yii官方文档：  
[YII AR 官方文档](http://www.yiiframework.com/doc-2.0/yii-db-activequery.html)