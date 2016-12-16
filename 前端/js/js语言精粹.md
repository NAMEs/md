##JS语言精粹笔记


1.如果第一个运算数的值为真,那么运算符||产生它的第一个运算数的值.否则,它产生第二个运算数的值.如,可利用||运算符来*填充默认值*

          var status = flight.status || "unkonwn"

2.如果第一个运算数的值为假,那么运算符&&产生它的第一个运算数的值.否则,它产生第二个运算数的值.如,可利用*&&运算符避免检索undefined引起的异常*；  

	flight.equipment                                             //undefined
	flight.equipment.model                                   //throw"TypeError"
	flight.equipment && flight.equipment.model     //undefined
	
3.6种值会为假(==false),分别是**false,null,undefined,' ',0,NaN**

4.原型机制有点像金字塔形状，上下的属性关系为**下面包含上面**，即上面有的属性下面一定有，下面有的上面不一定有；  
可以通过hasOwnProperty这个方法来判断一个对象的特殊的属性，特殊的属性的意思是不涉及该对象的原型；如：   

	var bStr = "Test String".hasOwnProperty("split");    // 得到false， 因为不能检测原型链中的属性 
	
但是实际上

	"Test String".split(" ");//是可以调用成功的


5.创建一个对象
可以通过对象字面量来创建一个对象；
如：  

	var empty_object = {};
	
	var stooge = {
	    "first-name": "Jerome",
	    "last-name": "Howard"
	};
	
对象的属性也可以是一个对象，只需在嵌套地定义相应的对象字面量；  

6.创建一个新对象，并制定一个对象作为它的原型  

	if (typeof Object.create !== 'function') {
	     Object.create = function (o) {
	         var F = function () {};
	         F.prototype = o;
	         return new F();
	     };
	}
	var another_stooge = Object.create(stooge);

