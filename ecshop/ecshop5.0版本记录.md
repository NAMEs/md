##快递接口
使用的是快递100，前端通过ajax中请求plugins/kuaidi100/kuaidi100_post.php，参数有快递名expressid和快递单号expressno；
所以可以不用迁顺丰快递接口

##支付接口情况





###支付宝情况
移动端支付情况： 

支付流程：  
在下单的时候可以选择支付方式，同时在下单后去我的订单那里支付也可以重新选择支付方式；  
订单详情里面走的流程是：  
先进入订单详情，然后点击立即支付的时候，弹框让用户选择支付方式，确认之后带了一个参数is\_pay，然后跳到order\_detail中，这时候前端就会判断是否带了is\_pay，如果带了就是提交id为zf\_button的那个跳转链接，这时候就跳到了支付宝或者微信的支付接口了；  
在微信中是可以使用支付宝支付的，普通的浏览器是不能使用微信支付的  

 

	/mobile\pay\alipayapi.php//这里是手机网页的支付文件，通过传过来的订单号，去后台查询价格    
	/mobile/pay/ajax_url.php//回调文件   
	/mobile/pay/result_url.php//结果页   
	
	\mobile\pay\alipayapiapp.php//红酒窝支付宝支付接口，  
	mobile/pay/ajax_url.php//回调地址  
	mobile/pay/result_url_app.php//结果显示  




###微信支付情况  

之前移动端支付情况：

	\mobile\pay\wxpay\jsapi.php  //微信内置浏览器使用jsapi进行支付的接口  
	/mobile/pay/wxpay/notify.php  //回调地址  
	
	\mobile\pay\wxpay\app_pay.php  //红酒窝app微信支付  
	/mobile/pay/wxpay/app_notify.php   //红酒窝微信支付回调接口  

5.0移动端支付情况

	mobile\weixinpay.php  //这里发起jsAPI支付请求的

	mobile\includes\modules\payment\weixin.php  //统一下单接口和回调地址

5.0PC支付情况：  

	includes\modules\payment\weixin.php   //跳到这里进行支付

	respond.php   //回调地址，这里的回调并没有进行异步处理，而是直接进行业务处理


##短信接口

注册（register.php）发送短信的调用的是sms/sms.php里面的sendSMS()方法；

后台发短信的基本都是调用send.php这个文件中的sendSMS()方法

