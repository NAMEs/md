#HTTP  
##请求
###格式
当浏览器向Web服务器发出请求时，它向服务器传递了一个数据块，也就是请求信息，  
HTTP请求信息由3部分组成：  
l   请求方法URI协议/版本  
l   请求头(Request Header)  
l   请求正文  

下面是一个HTTP请求的例子：  
GET/sample.jspHTTP/1.1  
Accept:image/gif.image/jpeg,*/*  
Accept-Language:zh-cn  
Connection:Keep-Alive  
Host:localhost  
User-Agent:Mozila/4.0(compatible;MSIE5.01;Window NT5.0)  
Accept-Encoding:gzip,deflate  
 
username=jinqiao&password=1234  
其中第一行GET/sample.jspHTTP/1.1 是请求方法URI协议/版本  
接下来到空行之前都是请求头的内容  
请求头和正文之间有一个空行进行区分隔开  

**HTTP应答码 ** 
HTTP应答码也称为状态码，它反映了Web服务器处理HTTP请求状态。HTTP应答码由3位数字构成，其中首位数字定义了应答码的类型：

1XX－信息类(Information),表示收到Web浏览器请求，正在进一步的处理中  
2XX－成功类（Successful）,表示用户请求被正确接收，理解和处理例如：200 OK   
3XX-重定向类(Redirection),表示请求没有成功，客户必须采取进一步的动作。  
4XX-客户端错误(Client Error)，表示客户端提交的请求有错误 例如：404 NOT Found，意味着请求中所引用的文档不存在。  
5XX-服务器错误(Server Error)表示服务器不能完成对请求的处理：如 500  
