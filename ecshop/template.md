##模板机制理解
在dwt文件中加入带有

	<!-- TemplateBeginEditable name="商品分类楼层1" -->

这样的代码，就会在后台被识别出来，具体还要配置一些东西，详情可以见网上的总结：
[http://www.360doc.com/content/14/0504/10/9200790_374419215.shtml](http://www.360doc.com/content/14/0504/10/9200790_374419215.shtml "ecshop模板机制总结")



模板的主要精华都在include/cls_template.php中，不清楚怎么解析模板的话，可以直接到display中查找，在fetch中
