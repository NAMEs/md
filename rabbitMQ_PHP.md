#connection and channel
connection是指物理的连接，一个client与一个server之间有一个连接；一个连接上可以建立多个channel，可以理解为逻辑上的连接。一般应用的情况下，有一个channel就够用了，不需要创建更多的channel。

	$conn = new AMQPConnection($conn_args);   
	if (!$conn->connect()) {   
	    die("Cannot connect to the broker!\n");   
	}   
	$channel = new AMQPChannel($conn);  
	##3，exchange 与  routingkey ： 交换机 与 路由键##

为了将不同类型的消息进行区分，设置了交换机与路由两个概念。比如，将A类型的消息发送到名为‘C1’的交换机，将类型为B的发送到'C2'的交换机。当客户端连接C1处理队列消息时，取到的就只是A类型消息。进一步的，如果A类型消息也非常多，需要进一步细化区分，比如某个客户端只处理A类型消息中针对K用户的消息，**routingkey**就是来做这个用途的。

	$e_name = 'e_linvo'; //交换机名 
	$k_route = array(0=> 'key_1', 1=> 'key_2'); //路由key 
	//创建交换机    
	$ex = new AMQPExchange($channel);   
	$ex->setName($e_name); 
	$ex->setType(AMQP_EX_TYPE_DIRECT); //direct类型  
	$ex->setFlags(AMQP_DURABLE); //持久化 
	echo "Exchange Status:".$ex->declare()."\n";   
	for($i=0; $i<5; ++$i){ 
	    echo "Send Message:".$ex->publish($message . date('H:i:s'), $k_route[i%2])."\n";  
	}



**原理图**  
  
![](http://images2015.cnblogs.com/blog/806920/201509/806920-20150926161722819-779629690.png)

其中  
vhost 虚拟主机  
producer 生产者（负责发送信息）  
channel 通道  
exchange 交换机   
>Exchange类似于数据通信网络中的交换机，提供消息路由策略。rabbitmq中，producer不是通过信道直接将消息发送给queue，而是先发送给Exchange。一个Exchange可以和多个Queue进行绑定，producer在传递消息的时候，会传递一个ROUTING_KEY，Exchange会根据这个ROUTING_KEY按照特定的路由算法，将消息路由给指定的queue。和Queue一样，Exchange也可设置为持久化，临时或者自动删除。
>交换机和队列之间通过bind方法绑定  

queue  队列  
consumer 消费者（负责接收信息）



Producer 要产生消息必须要创建一个 Exchange ，Exchange 用于转发消息，但是它不会做存储，如果没有 Queue bind 到 Exchange 的话，它会直接丢弃掉 Producer 发送过来的消息，当然如果消息总是发送过去就被直接丢弃那就没有什么意思了，一个 Consumer 想要接受消息的话，就要创建一个 Queue ，并把这个 Queue bind 到指定的 Exchange 上，然后 Exchange 会把消息转发到 Queue 那里，Queue 会负责存储消息，Consumer 可以通过主动 Pop 或者是 Subscribe 之后被动回调的方式来从 Queue 中取得消息。