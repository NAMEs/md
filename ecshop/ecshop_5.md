#ecshop 5.0 记录
##微分销记录
###用户扫码
1.关注事件  
在weixin/index.php中：  
a.扫码关注之后首先是进行weixin_user表的插入  
b.然后会在用户关注后进行推送，目前系统是推送[点击绑定]()的链接，而这里的链接是跳转到mobile/user.php?wxid=$wxid.   
c.之后还会进行其他处理：  
会根据微信推送过来的信息，获取扫码的用户的微信信息和二维码中的内容，里面有type这个字段，如果type是4的话，就会将扫码的用户和二维码的拥有者进行暂时的主从关系绑定；如果type是6的话……  
用户点击[点击绑定]()的话，跳到 mobile/user.php?wxid=$wxid，之后进行的是用户进行第三方登录的时候插入users表和绑定上级分销商（如果有进行二维码扫描的话）  


###分享链接
分享的链接地址是跳转到v\_user\_erweima.php这个文件  
这个文件的处理方式：  
如果进去的用户的用户id和链接中带的user\_id不一致的话（即不是分享链接的用户），或者是没登陆，直接插入或者更新一条数据到weixin\_user表中，如果没有登陆的话就会跳转到注册页面中，并且会带一个父id参数过去，这样注册的时候就会绑定相应的用户了



有两个表需要注意：  
1.weixin_user表，这个表是把商城用户和其微信号绑定在一起的表；  
2.users表，用户绑定上级用户就是在这里进行的




##微信授权获取用户信息部分（授权登录）

###手机端

点击我的的时候，先跳到user.php文件中，这是和平时一样的，然后再这个文件中进行是否是微信浏览器的判断，如果是的话就进行授权操作，如果用户授权就跳转到weixin_login.php文件中进行微信登录；  
之后再回到user.php中


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



##自动确认收货
后台在系统设置里可以设置自动确认收货时间；  
执行代码在\api\okgoods.php里执行；  
然后调用这个文件的话是在多个页面通过

	Ajax.call('api/okgoods.php', '', '', 'GET', 'JSON');

来调用执行的；





