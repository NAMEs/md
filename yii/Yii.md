#Yii知识点总结
Yii是一个php的MVC开发框架，相比thinkphp更为面向对象  

##view部分
###用activeFrom来形成表格
**例子**  

	<?php
	$form = ActiveForm:: begin([
	            'id' => 'edit-form',
	            'action' => Url::toRoute( 'doctor/edit')
	]);
	?>
	
	< div class="modal-content">
	    <div class="modal-header">
	        <button type= "button" class="close" data-dismiss= "modal" aria-label= "Close"><span aria-hidden= "true">&times;</ span>
			</button>
	        <h4 class= "modal-title" >Modal title</ h4>
	    </div >
	    <div class="modal-body">
	        <?= $form->field($model, 'id')->hiddenInput()->label(false) ?>
	        <?= $form->field($model, 'name') ?>
	        <?= $form->field($model, 'enabled' )->checkbox()->label('启用' ) ?>
	    </ div>
	    <div class="modal-footer">
	        <button type= "button" class="btn btn-default" data-dismiss= "modal">关闭 </button>
	        <?= Html ::submitButton('保存 ', ['class' => 'btn btn-primary']) ?>
	    </ div>
	</div >
	
	<?php ActiveForm ::end(); ?>

###加载js和css文件

前端调用依赖类

	DailyAsset::register ($this);//这里就是使用一些加载文件  

这里通过一个dailyasset类来加载一些js，css文件
这个类是继承AssetBundle类  

	<?php
	/**
	* Created by PhpStorm.
	* User: linpeiyu
	* Date: 2016-07-01
	* Time: 14:42
	*/
	
	namespace app\assets;
	
	use yii\web\AssetBundle;
	
	/**
	* @author Qiang Xue < qiang.xue@gmail.com>
	* @since 2.0
	*/
	class DailyAsset extends AssetBundle
	{
	    public $basePath = '@webroot';
	    public $baseUrl = '@web';
	
	    public $js = [
	        'js/daily.js',
	//        'js/jquery.js',
	//        'js/bootstrap.min.js'
	    ];
	    public $depends = [
	        'yii\web\YiiAsset',
	        'yii\bootstrap\BootstrapAsset',
	    ];
	
	}

如果需要在head中就加载文件的话，可以在这个类中添加一个属性：  

	public $jsOptions = ['position'=>View::POS_HEAD];

  
  
**单个依赖文件的全局js都会在其依赖变量的js之后再加载：即$js会在$depends之后加载；**  
  
yii默认模板中之所以在body中加载bs.js，是因为在：  

	<?php $this->endBody() ?>  

中进行依赖文件的加载的；  
  
  
如果找不到bootstrap.js是在哪里加载的，其实是main.php在调用NavBar中的run的时候进行加载的：  

	public function run()
	{
	    $tag = ArrayHelper ::remove($this->containerOptions, 'tag', 'div');
	    echo Html::endTag($tag);
	    if ($this->renderInnerContainer) {
	        echo Html ::endTag('div');
	    }
	    $tag = ArrayHelper ::remove($this->options, 'tag', 'nav');
	    echo Html::endTag($tag);
	    BootstrapPluginAsset ::register($this->getView());//这里进行bootstrap.js的加载的
	} 

###前端bs  nav+dropdown例子  
    <?php
    NavBar::begin(['options' => ['class' => 'blogCat']]);
    $item = [
        ['label' => '首页', 'url' => ['/site/index'] ,'options'=>['class'=>'active']]
    ];
    foreach($cat_list as $cat)
    {
        $sec_cat = [
            ['label' => '基本语法', 'url' => ['/site/index'] ]
        ];
        foreach($cat['sec_cat'] as $sec)
        {
            $sec_cat[] = ['label' => $sec['cat_name'], 'url' => '#'];

        }
        $item[] = [
            'label' => $cat['cat_name'],
            'url' => ['/cat/info/cat_id/'.$cat['id']],
            'options'=>['class'=>'dropdown'],
            'items'=>$sec_cat
        ];
    }
    echo Nav::widget([
        'items' => $item,
        'options' => ['class' => 'navbar-nav navbar-left'],
    ]);
    NavBar::end();
    ?>




##controller  
###返回json到ajax
首先在返回之前代码中添加返回头信息

	Yii::$app->response->format=Response::FORMAT_JSON;

返回的时候直接return数组就行了

	return ['code'=>false,'message'=>$msg];
js中

	$.ajax({
	    type: "POST",
	    data:$('#form').serialize(),//可以通过这样获取整个form里面的值
	    async: false,
	    dataType:'json',
	    error: function(request) {
	        alert（data.message）；
	    },
	    success: function(data) {
	        layer.msg(data.message,{icon:data.code?6:5,time:1000},function(){
	            alert(data.message);
	        });
	    }
	});



##model



##数据库查询语句

一般通过Active Record查询的返回的都是一个实例，而且这个实例对字段的要求很高，如果想要以数组的形式返回的话，可以使用**asArray()**，使用方法如下：  

	// 以数组而不是对象形式取回客户信息：
	$customers = Customer::find()
	    ->asArray()
	    ->all();
	// $customers 的每个元素都是键值对数组


#日志记录
yii中代码记录日志的做法为：

	Yii::getLogger()->log(“your site has been hacked”, Logger::LEVEL_ERROR) //默认category为application即应用程序级别的日志  



#错误记录
1. 更新数据的时候，可能会更新不了，save返回false;继承ActiveRecord的model的save是跳到BaseActiveRecord，但是BaseActiveRecord中save的update是跳到ActiveRecord那边，而在ActiveRecord的update中，就有进行字段有效性的检测，所以如果保存不了数据的话，就要看看model中的rules有进行了那些限制了；
2. 
  




#数据更新流程
