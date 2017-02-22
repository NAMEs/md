Yii 问题

今天遇到一个问题，抛出Class require does not exist问题，由于没有看下面的错误提示，所以一直找不到问题，直到看了问题提示：

	Stack trace:
	#0 /Library/WebServer/Documents/game_mugua/vendor/yiisoft/yii2/di/Container.php(424): ReflectionClass->__construct('require')
	#1 /Library/WebServer/Documents/game_mugua/vendor/yiisoft/yii2/di/Container.php(364): yii\di\Container->getDependencies('require')
	#2 /Library/WebServer/Documents/game_mugua/vendor/yiisoft/yii2/di/Container.php(156): yii\di\Container->build('require', Array, Array)
	#3 /Library/WebServer/Documents/game_mugua/vendor/yiisoft/yii2/BaseYii.php(344): yii\di\Container->get('require', Array, Array)
	#4 /Library/WebServer/Documents/game_mugua/vendor/yiisoft/yii2/validators/Validator.php(224): yii\BaseYii::createObject(Array)
	#5 /Library/WebServer/Documents/game_mugua/vendor/yiisoft/yii2/base/Model.php(447): yii\validators\Validator::createValidator('require', Object(backend\models\AnntypeForm), Array, Array)
	
之后发现是由于model里面的required写成require了
真是个坑爹的问题，所以说出错时看Stack trace是多么重要的；