##JS面向对象

###抽象类


在传统面向对象语言中，抽象类的虚方法必须先被声明，但可以在其他方法中被调用，而在js中，虚方法就可以看作该类中没有定义的方法，但已经通过this指针使用了。和传统面向对象不同的是，这里的虚方法不需经过声明，而直接使用了。这些方法将在派生类中实现；  

一个很漂亮的抽象类实例：  

	Object.extend = function(des,source){
		for(property in source)
		{
			des[property] = source[property];
		}
		return des;
	}
	Object.property.extend = function(object)
	{
		return Object.extend.apply(this,[this,object]);
		//这里调用Object的extend方法
	}
	
	function base(){}//没有 构造函数
	base.prototype={
		initialize:function(){
			this.oninit();//调用了一个虚方法
		}
	}
	
	function class1(){
		//构造函数
	}
	
	class1.prototype = (new base()).extend({
		oninit:function(){
			alert('got it');
		}
	});
	//这里的extens是先找Object.extend但是发现是要两个参数，并不满足；
	//所以向上找了Object.property.extend
	//这里class1继承了base，
	//而且这个base通过Object.extend方法后拥有这里的oninit方法
	
	class1.oninit();//输出 got it
	
这样，当class1的实例中调用继承得到的initialize方法时，就会自动执行派生类的oninit方法；
这里的抽象概念是，base实际上是没有oninit这个方法，而是在当一些实例继承base的时候进行实现（应该可以看成是实现）；	

	
	
	
	
