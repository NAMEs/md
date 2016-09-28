#js正则表达式
----
##例子

		var  parse_url = /^(?:([A-Za-z]+):)?(\/{0,3})([0-9.\-a-zA-z]+)
						(?::(\d+))?(?:\/([^?#]*))?(?:\?([^#]*))?(?:#(.*))?$/;
		var url = "http://www.ora.com:80/goodparts?q#fragment";
		var result = parse_url.exec(url);

结果：

		http://www.ora.com:80/goodparts?q#fragment
		http
		//
		www.ora.com
		80
		goodparts
		q
		fragment

***
