#padding、margin和border  
![](http://i.imgur.com/IzuRVPq.gif)  
  
W3C组织建议把所有网页上的对像都放在一个盒(box)中，设计师可以通过创建定义来控制这个盒的属性，这些对像包括段落、列表、标题、图片以及层。盒模型主要定义四个区域：**内容(content)、内边距(padding)、边框(border)和外边距(margin)**。  
  
下图能很好地理解这几个属性：  

![](http://i.imgur.com/PkGYZH8.jpg)  


##margin
包括margin-top、margin-right、margin-bottom、margin-left，控制块级元素之间的距离，它们是透明不可见的。根据上、 右、下、左的顺时针规则，可以写为 margin: 40px 40px 40px 40px;  

当上下、左右margin值分别一致, 可简写为:

	margin: 40px 40px;   

前一个40px代表上下margin值，后一个40px代表左右margin值。  
  
##padding  
包括padding-top、padding-right、padding-bottom、padding-left，控制块级元素内部，content与border之间的距离，代码的简写和margin是一样的；    


*注: 当你想让两个元素的content在垂直方向(vertically)分隔时，既可以选择padding-top/bottom，也可以选择margin-top/bottom，再此Ruthless建议你尽量使用padding-top/bottom来达到你的目的，这是因为css中存在Collapsing margins(折叠的margins)的现象。*


##总结
**详细说明如下：   
如果只提供一个，将用于全部的四条边；  
如果提供两个，第一个用于上－下，第二个用于左－右；   
如果提供三个，第一个用于上，第二个用于左－右，第三个用于下；   
如果提供全部四个参数值，将按上－右－下－左的顺序作用于四边。**