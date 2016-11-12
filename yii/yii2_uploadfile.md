#YII upload file  
  
##models  
		<?php
		/**
		 * Created by PhpStorm.
		 * User: linpeiyu
		 * Date: 2016-06-29
		 * Time: 10:45
		 */
		
		namespace app\models;
		
		use yii\base\Model;
		use yii\web\UploadedFile;
		
		class UploadForm extends Model
		{
		    /**
		     * @var UploadedFile
		     */
		    public $imageFile;
		
		    public function rules()
		    {
		        return [
		            [['imageFile'], 'file', 'skipOnEmpty' => false, 'extensions' => 'png,jpg,gif'],
		        ];
		    }
		
		    public function upload()
		    {
		//        die(json_encode($this->imageFile));
		        if ($this->validate()) {
		            $this->imageFile->saveAs('uploads/' . $this->imageFile->baseName . '.' . $this->imageFile->extension);
		            return true;
		        } else {
		            return false;
		        }
		    }
		}

  
##views  
		<?php
		/**
		 * Created by PhpStorm.
		 * User: linpeiyu
		 * Date: 2016-06-29
		 * Time: 10:47
		 */
		
		
		use yii\widgets\ActiveForm;
		?>
		
		<?php $form = ActiveForm::begin(['options' => ['enctype' => 'multipart/form-data']]) ?>
		
		    <?= $form->field($model, 'imageFile')->fileInput() ?>
		
		    <button>Submit</button>
		
		<?php ActiveForm::end() ?>

##controller

	    public function actionUpload()
	    {
	        $model = new UploadForm();
	
	        if (Yii::$app->request->isPost) {
	            $model->imageFile = UploadedFile::getInstance($model, 'imageFile');
	            if ($model->upload()) {
	                // 文件上传成功
	                return;
	            }
	        }
	
	        return $this->render('upload', ['model' => $model]);
	    }


##problem
###No1:  fileinfo
when the rules is :  

	    public function rules()
	    {
	        return [
	            [['imageFile'], 'file', 'skipOnEmpty' => false, 'extensions' => 'png,jpg,gif'],
	        ];
	    }
that may something wrong like that:  

	The fileinfo PHP extension is not installed.
that is because the php_fileinfo is not opened ,you should uncomment in your php.ini this line:  

	extension=php_fileinfo.dll