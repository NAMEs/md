#Yii缓存

##缓存组件

数据缓存需要缓存组件提供支持，它代表各种缓存存储器，例如内存，文件，数据库。  
缓存组件通常注册为应用程序组件，这样它们就可以在全局进行配置与访问。

	//代码演示了如何配置应用程序组件 cache 
	//使用两个 memcached 服务器
	'components' => [
	    'cache' => [
	        'class' => 'yii\caching\MemCache',
	        'servers' => [
	            [
	                'host' => 'server1',
	                'port' => 11211,
	                'weight' => 100,
	            ],
	            [
	                'host' => 'server2',
	                'port' => 11211,
	                'weight' => 50,
	            ],
	        ],
	    ],
	],
	//这样就可以通过Yii::$app->cache访问上面的缓存组件



##数据缓存
数据缓存是指将**一些 PHP 变量**存储到缓存中，使用时再从缓存中取回。它也是更高级缓存特性的基础，例如查询缓存和内容缓存。

	// 尝试从缓存中取回 $data ，这里的$cache指向缓存组件
	$data = $cache->get($key);
	 
	if ($data === false) {
	 
	    // $data 在缓存中没有找到，则重新计算它的值
	 
	    // 将 $data 存放到缓存供下次使用
	    $cache->set($key, $data);
	}
	 
	// 这儿 $data 可以使用了。
	
	
支持的缓存存储器
Yii 支持一系列缓存存储器，概况如下：

- [[yii\caching\ApcCache]]：使用 PHP **APC** 扩展。这个选项可以认为是集中式应用程序环境中（例如：单一服务器，没有独立的负载均衡器等）最快的缓存方案。  
- [[yii\caching\DbCache]]：使用一个**数据库的表**存储缓存数据。要使用这个缓存，你必须创建一个与 [[yii\caching\DbCache::cacheTable]] 对应的表。  
- [[yii\caching\DummyCache]]: 仅作为一个缓存占位符，不实现任何真正的缓存功能。**这个组件的目的是为了简化那些需要查询缓存有效性的代码。**例如，在开发中如果服务器没有实际的缓存支 持，用它配置一个缓存组件。一个真正的缓存服务启用后，可以再切换为使用相应的缓存组件。两种条件下你都可以使用同样的代码 Yii::$app->cache->get($key) 尝试从缓存中取回数据而不用担心 Yii::$app->cache 可能是 null。  
- [[yii\caching\FileCache]]：使用**标准文件存储缓存数据**。这个特别适用于缓存**大块数据**，例如一个整页的内容。  
- [[yii\caching\MemCache]]：使用 PHP **memcache** 和 memcached 扩展。这个选项被看作分布式应用环境中（例如：多台服务器，有负载均衡等）最快的缓存方案。  
- [[yii\redis\Cache]]：实现了一个基于 **Redis** 键值对存储器的缓存组件（需要 redis 2.6.12 及以上版本的支持 ）。  
- [[yii\caching\WinCache]]：使用 PHP **WinCache**（另可参考）扩展.  
- [[yii\caching\XCache]]：使用 PHP **XCache**扩展。  
- [[yii\caching\ZendDataCache]]：使用 **Zend Data Cache** 作为底层缓存媒介。  

使用技巧：  
可以在同一个应用程序中使用不同的缓存存储器。一个常见的策略是使用基于内存的缓存存储器存储小而常用的数据（例如：统计数据），使用基于文件或数据库的缓存存储器存储大而不太常用的数据（例如：网页内容）。
	
	
##缓存api

所有缓存组件都有同样的基类 [[yii\caching\Cache]] ，因此都支持如下 API：

- [[yii\caching\Cache::get()|get()]]：通过**一个指定的键（key）从缓存中取回一项数据**。如果该项数据**不存在于缓存中**或者已经**过期/失效**，则返回值 false。
- [[yii\caching\Cache::set()|set()]]：将**一项数据指定一个键**，存放到缓存中。
- [[yii\caching\Cache::add()|add()]]：如果缓存中**未找到该键**，则将指定数据存放到缓存中。(未找到才会添加)
- [[yii\caching\Cache::mget()|mget()]]：通过指定的**多个键**从缓存中取回多项数据。
- [[yii\caching\Cache::mset()|mset()]]：将**多项数据**存储到缓存中，每项数据对应一个键。
- [[yii\caching\Cache::madd()|madd()]]：将多项数据存储到缓存中，每项数据对应一个键。**如果某个键已经存在于缓存中，则该项数据会被跳过**。
- [[yii\caching\Cache::exists()|exists()]]：返回一个值，指明某个键**是否存在于缓存**中。
- [[yii\caching\Cache::delete()|delete()]]：通过一个键，**删除**缓存中对应的值。
- [[yii\caching\Cache::flush()|flush()]]：**删除缓存中的所有数据**。

由于 [[yii\caching\Cache]] 实现了 PHP ArrayAccess 接口，缓存组件也可以像数组那样使用：

	$cache['var1'] = $value1;  
	// 等价于： $cache->set('var1', $value1);
	$value2 = $cache['var2'];  
	// 等价于： $value2 = $cache->get('var2');


##缓存依赖

除了超时设置，缓存数据还可能受到缓存依赖的影响而失效。

例如，[[yii\caching\FileDependency]] 代表对一个文件修改时间的依赖。这个依赖条件发生变化也就意味着相应的文件已经被修改。
  
缓存依赖用 **[[yii\caching\Dependency]]** 的派生类所表示。当调用 [[yii\caching\Cache::set()|set()]] 在缓存中存储一项数据时，可以同时传递一个关联的缓存依赖对象。

	// 创建一个对 example.txt 文件修改时间的缓存依赖
	$dependency = new \yii\caching\FileDependency(['fileName' => 'example.txt']);
	 
	// 缓存数据将在30秒后超时
	// 如果 example.txt 被修改，它也可能被更早地置为失效状态。
	$cache->set($key, $data, 30, $dependency);
	 
	// 缓存会检查数据是否已超时。
	// 它还会检查关联的依赖是否已变化。
	// 符合任何一个条件时都会返回 false。
	$data = $cache->get($key);
	


下面是**可用的缓存依赖**的概况：

- [[yii\caching\ChainedDependency]]：如果依赖链上任何一个依赖产生变化，则依赖改变。
- [[yii\caching\DbDependency]]：如果指定 SQL 语句的查询结果发生了变化，则依赖改变。
- [[yii\caching\ExpressionDependency]]：如果指定的 PHP 表达式执行结果发生变化，则依赖改变。
- [[yii\caching\FileDependency]]：如果文件的最后修改时间发生变化，则依赖改变。
- [[yii\caching\GroupDependency]]：将一项缓存数据标记到一个组名，你可以通过调用 [[yii\caching\GroupDependency::invalidate()]] 一次性将相同组名的缓存全部置为失效状态。



##查询缓存

查询缓存是一个建立在数据缓存之上的特殊缓存特性。它用于**缓存数据库查询的结果**。

查询缓存需要一个 [[yii\db\Connection|数据库连接]] 和一个有效的 cache 应用组件。

	$duration = 60;     // 缓存查询结果60秒
	$dependency = ...;  // 可选的缓存依赖
	 
	$db->beginCache($duration, $dependency);
	 
	// ...这儿执行数据库查询...
	 
	$db->endCache();
	
*beginCache() 和 endCache() 中间的任何查询结果都会被缓存起来。如果缓存中找到了同样查询的结果，则查询会被跳过，直接从缓存中提取结果。*

###配置

查询缓存有两个通过 [[yii\db\Connection]] 设置的配置项：

- [[yii\db\Connection::queryCacheDuration|queryCacheDuration]]: 查询结果在缓存中的有效期，以秒表示。如果在调用 [[yii\db\Connection::beginCache()]] 时传递了一个显式的时值参数，则配置中的有效期时值会被覆盖。
- [[yii\db\Connection::queryCache|queryCache]]: 缓存应用组件的 ID。默认为 'cache'。只有在设置了一个有效的缓存应用组件时，查询缓存才会有效。


#片段缓存

片段缓存指的是缓存**页面内容（view）**中的某个片段。  
例如，一个页面显示了逐年销售额的摘要表格，可以把表格缓存下来，以消除每次请求都要重新生成表格的耗时。


在**视图view**中使用以下结构启用片段缓存：


	if ($this->beginCache($id)) {
	 
	    // ... 在此生成内容 ...
	 
	    $this->endCache();
	}
	
和[**数据缓存**]一样，每个片段缓存也需要全局唯一的 **$id** 标记。


##动态内容

使用片段缓存时，可能会遇到一大段较为静态的内容中有少许动态内容的情况。  
例如，一个显示着菜单栏和当前用户名的页面头部。还有一种可能是缓存的内容可能包含每次请求都需要执行的 PHP 代码（例如注册资源包的代码）。这两个问题都可以使用动态内容功能解决。

动态内容的意思是这部分输出的内容不该被缓存，即便是它被包裹在片段缓存中。为了使内容保持动态，**每次请求都执行 PHP 代码生成，即使这些代码已经被缓存了**。

	if ($this->beginCache($id1)) {
	 
	    // ...在此生成内容...
	 
	    echo $this->renderDynamic('return Yii::$app->user->identity->name;');//用户名是每次都不能缓存的，保证实时更新
	 
	    // ...在此生成内容...
	 
	    $this->endCache();
	}
	
[[yii\base\View::renderDynamic()|renderDynamic()]] 方法接受一段 **PHP 代码作为参数**。**代码的返回值被看作是动态内容**。这段代码将在每次请求时都执行，无论其外层的片段缓存是否被存储。


#页面缓存

页面缓存指的是在服务器端缓存整个页面的内容。  
随后当同一个页面被请求时，内容将从缓存中取出，而不是重新生成。  
页面缓存由 **[[yii\filters\PageCache]]** 类提供支持，该类是一个过滤器。

	public function behaviors()
	{
	    return [
	        [
	            'class' => 'yii\filters\PageCache',
	            'only' => ['index'],
	            'duration' => 60,
	            'variations' => [
	                \Yii::$app->language,
	            ],
	            'dependency' => [
	                'class' => 'yii\caching\DbDependency',
	                'sql' => 'SELECT COUNT(*) FROM post',
	                //这里表示如果文章总数发生变化则缓存会失效
	            ],
	        ],
	    ];
	}

上述代码表示页面缓存只在 index 操作时启用，页面内容最多被缓存 60 秒，会随着当前应用的语言更改而变化。**如果文章总数发生变化则缓存的页面会失效。**

页面缓存和片段缓存极其相似。它们都支持 **duration**，**dependencies**，**variations** 和 **enabled** 配置选项。*它们的主要区别是页面缓存是由过滤器实现，而片段缓存则是一个小部件*。


	
