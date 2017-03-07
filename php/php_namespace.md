##namespace

the php namespace is only use to recognize the Class with the same name ;  
and the "use" does not mean having loaded the Class file ,it just means we can use the Class form that namespace;  
for example:  

	namespace app\models;
	use app\models\Test;
	include_once ("Test.php");
	include_once ("../View/Test.php");
	
	class A extends Test{//这里由于上面的use ，所以表示的是当前目录下的Test.php文件里的类
		
	}
	
	class B extends \app\view\Test{
	
	}
	
命名空间只是用来识别同名但是不同位置的Class；  
而use则是起了一个**别名**的作用，意思是：use app\models\Test as Test;  
但是注意的是use 并没有实际引入这个Class文件，还是必须使用include 来引入，所以实际上的use 只是起了一个别名的作用，在同一个文件中引入多个拥有同样名字类的文件时会起到作用；