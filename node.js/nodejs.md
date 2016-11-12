##环境搭建
安装node.js，在官网下载安装，然后用命令行cd到安装目录，进行环境配置；
###安装express  
	npm install express -gd  
其中，-g代表安装到NODE_PATH的lib里面，而-d代表把相依性套件也一起安装。如果沒有-g的话会安装目前所在的目录(会建立一个node_modules的文件夹)。  
如果出现了express不是内部或外部命令的话，可以安装express-generator；  

	npm install -g express-generator  
###安装jade  
	npm install jade  
###安装mysql
	npm install mysql
###创建一个工程
	express myapp(project name)





#爬虫简单代码

	var express = require('exress');
	var app = express();
	var request = require('request');
	var cheerio = require('cheerio');
	app.get('/',function(req,res){
		request('http://www.baidu.com',function(error,response,body){
		if(!error&&response.statusCode == 200)
		{
			$ = cheerio.load(body);
			res.json({
			'clossnum':$('.class li').length
			});
		}
	});
	});
	app.listen(3000);