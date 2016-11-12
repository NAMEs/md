#yii前端view常用技巧总结  
##修改activeform的样式
	<?= $form->field($model,"content")->
		textInput(['maxlength' => 255,'placeholder'=>'请输入专题链接','style'=>"height:100px;"])
		->hint('Please enter your daily content here'); 
	?>
在input()中可以添加各种属性，包括样式style；

##yii显示其他的view

###in view

	<?= $this->render('@app/views/layouts/urhere.php')?>
###in controller

	echo $this->renderFile('@app/views/layouts/displaymsg.php', ['status' => $status, 'msg' => $msg, 'str' => $str]);
	
##前端导航栏使用bs插件

代码如下：如果要在导航栏中加一个下拉框，只需在一个item中继续加items：
  
	    <?php
	    NavBar::begin([
	        'brandLabel' => 'My Company',
	        'brandUrl' => Yii::$app->homeUrl,
	        'options' => [
	            'class' => 'navbar-inverse navbar-fixed-top',
	        ],
	    ]);
	    $menuItems = [
	        ['label' => 'Home', 'url' => ['/site/index']],
	    ];
	    if (Yii::$app->user->isGuest) {
	        $menuItems[] = ['label' => 'Login', 'url' => ['/site/login']];
	
	        $menuItems[] = [
	            'label'=>'公告管理',
	            'items'=>[
	                   ['label' => '公告列表', 'url' => 'index.php?r=announcement/list'],
	//                   '<li class="divider"></li>',
	//                   '<li class="dropdown-header">Dropdown Header</li>',
	                   ['label' => '公告类型', 'url' => 'index.php?r=announcement/typelist'],
	              ],
	        ];
	    } else {
	
	        $menuItems[] = '<li>'
	            . Html::beginForm(['/site/logout'], 'post')
	            . Html::submitButton(
	                'Logout (' . Yii::$app->user->identity->admin_name . ')',
	                ['class' => 'btn btn-link']
	            )
	            . Html::endForm()
	            . '</li>';
	    }
	    echo Nav::widget([
	        'options' => ['class' => 'navbar-nav navbar-right'],
	        'items' => $menuItems,
	    ]);
	    NavBar::end();
	    ?>
	    
注意nav是在navbar里面，实现导航条的样式；
	    
	    
另外，可以参考bs插件中的例子进行编码，nav例子代码如下：   

	 * ```php
	 * echo Nav::widget([
	 *     'items' => [
	 *         [
	 *             'label' => 'Home',
	 *             'url' => ['site/index'],
	 *             'linkOptions' => [...],
	 *         ],
	 *         [
	 *             'label' => 'Dropdown',
	 *             'items' => [
	 *                  ['label' => 'Level 1 - Dropdown A', 'url' => '#'],
	 *                  '<li class="divider"></li>',
	 *                  '<li class="dropdown-header">Dropdown Header</li>',
	 *                  ['label' => 'Level 1 - Dropdown B', 'url' => '#'],
	 *             ],
	 *         ],
	 *         [
	 *             'label' => 'Login',
	 *             'url' => ['site/login'],
	 *             'visible' => Yii::$app->user->isGuest
	 *         ],
	 *     ],
	 *     'options' => ['class' =>'nav-pills'], // set this to nav-tab to get tab-styled navigation
	 * ]);
	 * ```

