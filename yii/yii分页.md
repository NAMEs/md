#Yii2分页

后端：  
 后端进行的处理是将数据库查询到的数据进行分页处理；  
 代码：
Model：  

        $pagination = new Pagination(['totalCount' => $count,'pageSize'=>1,'params'=>array_merge($_GET, ['keywords' => 'test'])]);
        //这里加了一个自定义参数，其实默认是用GET的所有值的，所以其实可以在前面直接把‘test’给到GET的一个字段的，

        $result['list'] = $articles = $query->offset($pagination->offset)
            ->limit($pagination->limit)
            ->all();
        $result['page'] = $pagination;
        
这一部分主要是将数据库的数据进行一个分页处理；
        
Controller：  

	$ann = new Announcement();
	$list = $ann->getList($filter);
	$content = $this->render('list',['lists'=>$list['list'],'pagination'=>$list['page']]);
	
控制器部分主要是进行数据的传送，传到前端；

View：

	<div style="text-align: center">
	    <?php
	    echo \yii\widgets\LinkPager::widget([
	        'pagination' => $pagination,
	    ]);
	    ?>
	</div>

这一部分是在直接在前端生成那排按钮，每个按钮默认继承之前的get参数，也可以进行自定义参数