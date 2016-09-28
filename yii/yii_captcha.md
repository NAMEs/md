#YII captcha
##controller
    public function actions()
    {
        return [
            'captcha' => [
                'class' => 'yii\captcha\CaptchaAction',
                'maxLength' => 5,
                'minLength' => 5
            ],
        ];
    }
actions()方法是用来定义一些已经有的小部件，这些小部件就相当于一个普通的actions来用  
对应的控制器必须要有一个actions()方法，并且在前端view中也要将验证码的路由引导到这个controller中，这样前端的验证码才能正确显示；
##models

    public $verifyCode;

    public function rules()
    {
        return [
            ['verifyCode', 'required'],
            ['verifyCode', 'captcha'],
            [['imageFile'], 'file', 'skipOnEmpty' => false, 'extensions' => 'png,jpg,gif'],
        ];
    }  
需要注意的是，要定义变量，代表表单提交的某个值，rules可以查看资料http://www.yiichina.com/doc/guide/2.0/tutorial-core-validators#file  
##view
		<?= $form->field($model, 'verifyCode', [
		    'options' => ['class' => 'form-group form-group-lg'],
		])->widget(Captcha::className(),[
		    'template' => "{input}{image}",
		    'imageOptions' => ['alt' => '验证码'],
		    'captchaAction' => 'user/captcha',
		]); ?>
需要注意的是{input}表示输入框，{image}表示验证码图片；