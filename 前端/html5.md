#html5基础

##声明
<!doctype> 声明必须位于 **HTML5 文档中的第一行**

	<!DOCTYPE html>

##最小的html5样板

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="utf-8">
	<title>文档标题</title>
	</head>
	 
	<body>
	文档内容......
	</body>
	 
	</html>

##新特性

HTML5 中的一些有趣的新特性：

- 用于绘画的 canvas 元素
- 用于媒介回放的 video 和 audio 元素
- 对本地离线存储的更好的支持
- 新的特殊内容元素，比如 article、footer、header、nav、section
- 新的表单控件，比如 calendar、date、time、email、url、search

----------

#html5实例

##视频（video）

	<video width="320" height="240" controls="controls">
	  <source src="movie.ogg" type="video/ogg">
	  <source src="movie.mp4" type="video/mp4"> <!--这里是 提供给支持video但是不支持ogg格式的浏览器显示-->
	Your browser does not support the video tag.<!--这里是 提供给不支持video的浏览器显示-->
	</video>


- control 属性供添加播放、暂停和音量控件。
- video 元素允许多个 source 元素。source 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式

参数：

    | 属性        | 值    |  描述  |
    | --------   | -----:   | :------------------------: |
    | autoplay        | autoplay      |   如果出现该属性，则视频在就绪后马上播放。    |
    | controls        | controls      |   如果出现该属性，则向用户显示控件，比如播放按钮。    |
    | loop        | loop      |   如果出现该属性，则当媒介文件完成播放后再次开始播放。    |
	| preload        | preload      |   	如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。    |
	| src        | **url**      |   要播放的视频的 URL。    |

----------

##SVG(矢量图)

代码：

	<!DOCTYPE html>
	<html>
	<body>
	 
	<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
	  <polygon points="100,10 40,180 190,60 10,60 160,180"
	  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;">
	</svg>
	 
	</body>
	</html>







