##导航栏nav
###默认形式
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 默认的导航栏</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<nav class="navbar navbar-default" role="navigation">
	   <div class="navbar-header">
	      <a class="navbar-brand" href="#">W3Cschool</a>
	   </div>
	   <div>
	      <ul class="nav navbar-nav">
	         <li class="active"><a href="#">iOS</a></li>
	         <li><a href="#">SVN</a></li>
	         <li class="dropdown">
	            <a href="#" class="dropdown-toggle" data-toggle="dropdown">
	               Java 
	               <b class="caret"></b>
	            </a>
	            <ul class="dropdown-menu">
	               <li><a href="#">jmeter</a></li>
	               <li><a href="#">EJB</a></li>
	               <li><a href="#">Jasper Report</a></li>
	               <li class="divider"></li>
	               <li><a href="#">分离的链接</a></li>
	               <li class="divider"></li>
	               <li><a href="#">另一个分离的链接</a></li>
	            </ul>
	         </li>
	      </ul>
	   </div>
	</nav>
	
	
	</body>
	</html>  
  
###响应式
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 响应式的导航栏</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<nav class="navbar navbar-default" role="navigation">
	   <div class="navbar-header">
	      <button type="button" class="navbar-toggle" data-toggle="collapse" 
	         data-target="#example-navbar-collapse">
	         <span class="sr-only">切换导航</span>
	         <span class="icon-bar"></span>
	         <span class="icon-bar"></span>
	         <span class="icon-bar"></span>
	      </button>
	      <a class="navbar-brand" href="#">W3Cschool</a>
	   </div>
	   <div class="collapse navbar-collapse" id="example-navbar-collapse">
	      <ul class="nav navbar-nav">
	         <li class="active"><a href="#">iOS</a></li>
	         <li><a href="#">SVN</a></li>
	         <li class="dropdown">
	            <a href="#" class="dropdown-toggle" data-toggle="dropdown">
	               Java <b class="caret"></b>
	            </a>
	            <ul class="dropdown-menu">
	               <li><a href="#">jmeter</a></li>
	               <li><a href="#">EJB</a></li>
	               <li><a href="#">Jasper Report</a></li>
	               <li class="divider"></li>
	               <li><a href="#">分离的链接</a></li>
	               <li class="divider"></li>
	               <li><a href="#">另一个分离的链接</a></li>
	            </ul>
	         </li>
	      </ul>
	   </div>
	</nav>
	
	
	</body>
	</html>
##模态框Modal
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 模态框（Modal）插件方法</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<h2>模态框（Modal）插件方法</h2>
	
	<!-- 按钮触发模态框 -->
	<button class="btn btn-primary btn-lg" data-toggle="modal" data-target="#myModal"><!--这里要注意-->
	   开始演示模态框
	</button>
	
	<!-- 模态框（Modal） -->
	<div class="modal fade" id="myModal" tabindex="-1" role="dialog" 
	   aria-labelledby="myModalLabel" aria-hidden="true">
	   <div class="modal-dialog">
	      <div class="modal-content">
	         <div class="modal-header">
	            <button type="button" class="close" data-dismiss="modal" 
	               aria-hidden="true">×
	            </button>
	            <h4 class="modal-title" id="myModalLabel">
	               模态框（Modal）标题
	            </h4>
	         </div>
	         <div class="modal-body">
	            按下 ESC 按钮退出。
	         </div>
	         <div class="modal-footer">
	            <button type="button" class="btn btn-default" 
	               data-dismiss="modal">关闭
	            </button>
	            <button type="button" class="btn btn-primary">
	               提交更改
	            </button>
	         </div>
	      </div><!-- /.modal-content -->
	   </div><!-- /.modal-dialog -->
	</div><!-- /.modal -->
	<script>
	   $(function () { $('#myModal').modal({
	      keyboard: true
	   })});
	</script>
	
	</body>
	</html>

##下拉框Dropdown
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 带有下拉菜单的标签页</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<p>带有下拉菜单的标签页</p>
	<ul class="nav nav-tabs">
	   <li class="active"><a href="#">Home</a></li>
	   <li><a href="#">SVN</a></li>
	   <li><a href="#">iOS</a></li>
	   <li><a href="#">VB.Net</a></li>
	   <li class="dropdown">
	      <a class="dropdown-toggle" data-toggle="dropdown" href="#">
	         Java <span class="caret"></span>
	      </a>
	      <ul class="dropdown-menu">
	         <li><a href="#">Swing</a></li>
	         <li><a href="#">jMeter</a></li>
	         <li><a href="#">EJB</a></li>
	         <li class="divider"></li>
	         <li><a href="#">分离的链接</a></li>
	      </ul>
	   </li>
	   <li><a href="#">PHP</a></li>
	</ul>
	
	</body>
	</html>


##滚动监听  
&nbsp;&nbsp;&nbsp;&nbsp;向您想要监听的元素（通常是 body）添加 data-spy="**scroll**"。然后添加带有 Bootstrap .nav 组件的父元素的 ID 或 class 的属性 data-target。
###data类型
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 滚动监听（Scrollspy）插件</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<nav id="navbar-example" class="navbar navbar-default navbar-static" role="navigation">
	   <div class="navbar-header">
	      <button class="navbar-toggle" type="button" data-toggle="collapse" 
	         data-target=".bs-js-navbar-scrollspy">
	         <span class="sr-only">切换导航</span>
	         <span class="icon-bar"></span>
	         <span class="icon-bar"></span>
	         <span class="icon-bar"></span>
	      </button>
	      <a class="navbar-brand" href="#">教程名称</a>
	   </div>
	   <div class="collapse navbar-collapse bs-js-navbar-scrollspy">
	      <ul class="nav navbar-nav">
	         <li><a href="#ios">iOS</a></li>
	         <li><a href="#svn">SVN</a></li>
	         <li class="dropdown">
	            <a href="#" id="navbarDrop1" class="dropdown-toggle" 
	               data-toggle="dropdown">Java
	               <b class="caret"></b>
	            </a>
	            <ul class="dropdown-menu" role="menu" 
	               aria-labelledby="navbarDrop1">
	               <li><a href="#jmeter" tabindex="-1">jmeter</a></li>
	               <li><a href="#ejb" tabindex="-1">ejb</a></li>
	               <li class="divider"></li>
	               <li><a href="#spring" tabindex="-1">spring</a></li>
	            </ul>
	         </li>
	      </ul>
	   </div>
	</nav>
	<div data-spy="scroll" data-target="#navbar-example" data-offset="0" 
	   style="height:200px;overflow:auto; position: relative;">
	   <h4 id="ios">iOS</h4>
	   <p>iOS 是一个由苹果公司开发和发布的手机操作系统。最初是于 2007 年首次发布 iPhone、iPod Touch 和 Apple 
	      TV。iOS 派生自 OS X，它们共享 Darwin 基础。OS X 操作系统是用在苹果电脑上，iOS 是苹果的移动版本。
	   </p>
	   <h4 id="svn">SVN</h4>
	   <p>Apache Subversion，通常缩写为 SVN，是一款开源的版本控制系统软件。Subversion 由 CollabNet 公司在 2000 年创建。但是现在它已经发展为 Apache Software Foundation 的一个项目，因此拥有丰富的开发人员和用户社区。
	   </p>
	   <h4 id="jmeter">jMeter</h4>
	   <p>jMeter 是一款开源的测试软件。它是 100% 纯 Java 应用程序，用于负载和性能测试。
	   </p>
	   <h4 id="ejb">EJB</h4>
	   <p>Enterprise Java Beans（EJB）是一个创建高度可扩展性和强大企业级应用程序的开发架构，部署在兼容应用程序服务器（比如 JBOSS、Web Logic 等）的 J2EE 上。
	   </p>
	   <h4 id="spring">Spring</h4>
	   <p>Spring 框架是一个开源的 Java 平台，为快速开发功能强大的 Java 应用程序提供了完备的基础设施支持。
	   </p>
	   <p>Spring 框架最初是由 Rod Johnson 编写的，在 2003 年 6 月首次发布于 Apache 2.0 许可证下。
	   </p>
	</div>
	
	</body>
	</html>

###水平监听
	<!DOCTYPE html>
	<html lang="en">
	<head>
	  <title>Bootstrap Example</title>
	  <meta charset="utf-8">
	  <meta name="viewport" content="width=device-width, initial-scale=1">
	  <link href="http://libs.baidu.com/bootstrap/3.3.0/css/bootstrap.min.css" rel="stylesheet">
	  <script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
	  <script src="http://libs.baidu.com/bootstrap/3.3.0/js/bootstrap.min.js"></script>
	  <style>
	  body {
	      position: relative;
	  }
	  ul.nav-pills {
	      top: 20px;
	      position: fixed;
	  }
	  div.col-sm-9 div {
	      height: 250px;
	      font-size: 28px;
	  }
	  #section1 {color: #fff; background-color: #1E88E5;}
	  #section2 {color: #fff; background-color: #673ab7;}
	  #section3 {color: #fff; background-color: #ff9800;}
	  #section41 {color: #fff; background-color: #00bcd4;}
	  #section42 {color: #fff; background-color: #009688;}
	  
	  @media screen and (max-width: 810px) {
	    #section1, #section2, #section3, #section41, #section42  {
	        margin-left: 150px;
	    }
	  }
	  </style>
	</head>
	<body data-spy="scroll" data-target="#myScrollspy" data-offset="20">
	
	<div class="container">
	  <div class="row">
	    <nav class="col-sm-3" id="myScrollspy">
	      <ul class="nav nav-pills nav-stacked">
	        <li class="active"><a href="#section1">Section 1</a></li>
	        <li><a href="#section2">Section 2</a></li>
	        <li><a href="#section3">Section 3</a></li>
	        <li class="dropdown">
	          <a class="dropdown-toggle" data-toggle="dropdown" href="#">Section 4 <span class="caret"></span></a>
	          <ul class="dropdown-menu">
	            <li><a href="#section41">Section 4-1</a></li>
	            <li><a href="#section42">Section 4-2</a></li>                     
	          </ul>
	        </li>
	      </ul>
	    </nav>
	    <div class="col-sm-9">
	      <div id="section1">    
	        <h1>Section 1</h1>
	        <p>Try to scroll this section and look at the navigation list while scrolling!</p>
	      </div>
	      <div id="section2"> 
	        <h1>Section 2</h1>
	        <p>Try to scroll this section and look at the navigation list while scrolling!</p>
	      </div>        
	      <div id="section3">         
	        <h1>Section 3</h1>
	        <p>Try to scroll this section and look at the navigation list while scrolling!</p>
	      </div>
	      <div id="section41">         
	        <h1>Section 4-1</h1>
	        <p>Try to scroll this section and look at the navigation list while scrolling!</p>
	      </div>      
	      <div id="section42">         
	        <h1>Section 4-2</h1>
	        <p>Try to scroll this section and look at the navigation list while scrolling!</p>
	      </div>
	    </div>
	  </div>
	</div>
	
	</body>
	</html> 

##轮播效果插件
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 简单的轮播（Carousel）插件</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<div id="myCarousel" class="carousel slide">
	   <!-- 轮播（Carousel）指标 -->
	   <ol class="carousel-indicators">
	      <li data-target="#myCarousel" data-slide-to="0" class="active"></li>
	      <li data-target="#myCarousel" data-slide-to="1"></li>
	      <li data-target="#myCarousel" data-slide-to="2"></li>
	   </ol>   
	   <!-- 轮播（Carousel）项目 -->
	   <div class="carousel-inner">
	      <div class="item active">
	         <img src="/wp-content/uploads/2014/07/slide1.png" alt="First slide">
	      </div>
	      <div class="item">
	         <img src="/wp-content/uploads/2014/07/slide2.png" alt="Second slide">
	      </div>
	      <div class="item">
	         <img src="/wp-content/uploads/2014/07/slide3.png" alt="Third slide">
	      </div>
	   </div>
	   <!-- 轮播（Carousel）导航 -->
	   <a class="carousel-control left" href="#myCarousel" 
	      data-slide="prev">&lsaquo;</a>
	   <a class="carousel-control right" href="#myCarousel" 
	      data-slide="next">&rsaquo;</a>
	</div> 
	
	</body>
	</html>
                
##标签
	<!DOCTYPE html>
	<html>
	<head>
	   <title>Bootstrap 实例 - 标签页（Tab）插件</title>
	   <link href="/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	   <script src="/scripts/jquery.min.js"></script>
	   <script src="/bootstrap/js/bootstrap.min.js"></script>
	</head>
	<body>
	
	<ul id="myTab" class="nav nav-tabs">
	   <li class="active">
	      <a href="#home" data-toggle="tab">
	         W3Cschool Home
	      </a>
	   </li>
	   <li><a href="#ios" data-toggle="tab">iOS</a></li>
	   <li class="dropdown">
	      <a href="#" id="myTabDrop1" class="dropdown-toggle" 
	         data-toggle="dropdown">Java 
	         <b class="caret"></b>
	      </a>
	      <ul class="dropdown-menu" role="menu" aria-labelledby="myTabDrop1">
	         <li><a href="#jmeter" tabindex="-1" data-toggle="tab">jmeter</a></li>
	         <li><a href="#ejb" tabindex="-1" data-toggle="tab">ejb</a></li>
	      </ul>
	   </li>
	</ul>
	<div id="myTabContent" class="tab-content">
	   <div class="tab-pane fade in active" id="home">
	      <p>W3Cschoool菜鸟教程是一个提供最新的web技术站点，本站免费提供了建站相关的技术文档，帮助广大web技术爱好者快速入门并建立自己的网站。菜鸟先飞早入行——学的不仅是技术，更是梦想。</p>
	   </div>
	   <div class="tab-pane fade" id="ios">
	      <p>iOS 是一个由苹果公司开发和发布的手机操作系统。最初是于 2007 年首次发布 iPhone、iPod Touch 和 Apple 
	      TV。iOS 派生自 OS X，它们共享 Darwin 基础。OS X 操作系统是用在苹果电脑上，iOS 是苹果的移动版本。</p>
	   </div>
	   <div class="tab-pane fade" id="jmeter">
	      <p>jMeter 是一款开源的测试软件。它是 100% 纯 Java 应用程序，用于负载和性能测试。</p>
	   </div>
	   <div class="tab-pane fade" id="ejb">
	      <p>Enterprise Java Beans（EJB）是一个创建高度可扩展性和强大企业级应用程序的开发架构，部署在兼容应用程序服务器（比如 JBOSS、Web Logic 等）的 J2EE 上。
	      </p>
	   </div>
	</div>
	
	</body>
	</html>                     
