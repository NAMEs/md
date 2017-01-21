#YII权限控制

##存取控制过滤器（ACF）

通过 yii\filters\AccessControl 类来实现的简单授权方法。主要的功能是判断是否用户是否是已经登陆了，配置已经登陆了的用户能访问的action；

例子代码：   

	use yii\web\Controller;
	use yii\filters\AccessControl;
	
	class SiteController extends Controller
	{
	    public function behaviors()
	    {
	        return [
	            'access' => [
	                'class' => AccessControl::className(),
	                'only' => ['login', 'logout', 'signup'],
	                'rules' => [
	                    [
	                        'allow' => true,
	                        'actions' => ['login', 'signup'],
	                        'roles' => ['?'],
	                    ],
	                    [
	                        'allow' => true,
	                        'actions' => ['logout'],
	                        'roles' => ['@'],
	                    ],
	                ],
	            ],
	        ];
	    }
	    // ...
	}
	
其中，roles包括两种，一种是？表示访客权限，@表示已经登陆的用户权限；
上面的规则是：只有login、logout和signup这三个action需要权限验证，其中的login和signup是针对游客权限进行验证；而logout是针对已经登陆的用户进行验证；
  

###验证失败处理
当 ACF 判定一个用户没有获得执行当前操作的授权时，它的默认处理是：  
如果该用户是访客，将调用 yii\web\User::loginRequired() 将用户的浏览器重定向到登录页面。  
如果该用户是已认证用户，将抛出一个 yii\web\ForbiddenHttpException 异常。


##基于角色的存取控制 （RBAC）

使用 RBAC 涉及到两部分工作：  
第一部分是建立授权数据；  
第二部分是使用这些授权数据在需要的地方执行检查。

###角色
角色是权限的集合，一个角色可以指派给多个用户；
###规则rules
可以用一个rules和一个角色或者权限关联，用于验证一个用户是否满足这个角色或者权限（规则由自己定义，传给执行规则的方法的参数也是由自己定义）；

配置rbac数据库：

	return [
	    // ...
	    'components' => [
	        'authManager' => [
	            'class' => 'yii\rbac\DbManager',
	        ],
	        // ...
	    ],
	];
	
Note: If you are using yii2-basic-app template, there is a config/console.php configuration file where the authManager needs to be declared additionally to config/web.php. In case of yii2-advanced-app the authManager should be declared only once in common/config/main.php.

数据库需要使用四个表

	yii\rbac\DbManager::$itemTable： 该表存放授权条目（译者注：即角色和权限）。默认表名为 "auth_item" 。
	yii\rbac\DbManager::$itemChildTable： 该表存放授权条目的层次关系。默认表名为 "auth_item_child"。
	yii\rbac\DbManager::$assignmentTable： 该表存放授权条目对用户的指派情况。默认表名为 "auth_assignment"。
	yii\rbac\DbManager::$ruleTable： 该表存放规则。默认表名为 "auth_rule"。
	
创建这些表可以通过数据库迁移的方式，同时把权限数据建立好了之后再迁移过去；  
数据库迁移命令：   

	yii migrate --migrationPath=@yii/rbac/migrations
	
之后就可以通过：  

	\Yii::$app->authManager
	
访问authManager了

接下来是代码解释：

	<?php
	namespace app\commands;//如果是高级模版的话这里需要修改，需要转到console/controllers中
	
	use Yii;
	use yii\console\Controller;
	
	class RbacController extends Controller
	{
	    public function actionInit()
	    {
	        $auth = Yii::$app->authManager;
	
	        // 添加 "createPost" 权限
	        $createPost = $auth->createPermission('createPost');
	        $createPost->description = 'Create a post';
	        $auth->add($createPost);
	
	        // 添加 "updatePost" 权限
	        $updatePost = $auth->createPermission('updatePost');
	        $updatePost->description = 'Update post';
	        $auth->add($updatePost);
	
	        // 添加 "author" 角色并赋予 "createPost" 权限
	        $author = $auth->createRole('author');
	        $auth->add($author);
	        $auth->addChild($author, $createPost);
	
	        // 添加 "admin" 角色并赋予 "updatePost" 
			// 和 "author" 权限
	        $admin = $auth->createRole('admin');
	        $auth->add($admin);
	        $auth->addChild($admin, $updatePost);
	        $auth->addChild($admin, $author);//这里包含了author角色
	
			//指派权限部分通常是在其他代码里面，比如用户注册的时候就指派一个角色给那个用户；
	        // 为用户指派角色。其中 1 和 2 是由 IdentityInterface::getId() 返回的id （译者注：user表的id）
	        // 通常在你的 User 模型中实现这个函数。
	        $auth->assign($author, 2);
	        $auth->assign($admin, 1);
	    }
	}  
	
这部分代码只是展示怎样创建角色和权限并指派权限给角色，  
其中的指派角色给用户这一操作实际上是会在代码中的其他地方进行的；
如以下代码：  

	public function signup()
	{
	    if ($this->validate()) {
	        $user = new User();
	        $user->username = $this->username;
	        $user->email = $this->email;
	        $user->setPassword($this->password);
	        $user->generateAuthKey();
	        $user->save(false);
	
	        // 要添加以下三行代码：
	        $auth = Yii::$app->authManager;
	        $authorRole = $auth->getRole('author');//获取角色
	        
	        
	        $auth->assign($authorRole, $user->getId());
	        //这里进行了用户角色指派操作
	
	        return $user;
	    }
	
	    return null;
	}
	
	
规则部分代码（rules）  
规则给角色和权限增加额外的约束条件。  
规则是 yii\rbac\Rule 的派生类。 它需要实现 yii\rbac\Rule::execute() 方法。  

下面实现一个author对自己创建的帖进行修改更新：

	namespace app\rbac;
	
	use yii\rbac\Rule;
	
	/**
	 * 检查 authorID 是否和通过参数传进来的 user 参数相符
	 */
	class AuthorRule extends Rule
	{
	    public $name = 'isAuthor';
	
	    /**
	     * @param string|integer $user 用户 ID.
	     * @param Item $item 该规则相关的角色或者权限
	     * @param array $params 传给 ManagerInterface::checkAccess() 的参数
	     * @return boolean 代表该规则相关的角色或者权限是否被允许
	     */
	     //这里的$params是验证的时候传过来的参数
	    public function execute($user, $item, $params)
	    {
	        return isset($params['post']) ? $params['post']->createdBy == $user : false;
	    }
	}
	
上述规则检查 post 是否是 $user 创建的。我们还要在之前的命令中 创建一个特别的权限 updateOwnPost ：
	
	$auth = Yii::$app->authManager;
	
	// 添加规则
	$rule = new \app\rbac\AuthorRule;
	$auth->add($rule);
	
	// 添加 "updateOwnPost" 权限并与规则关联
	$updateOwnPost = $auth->createPermission('updateOwnPost');
	$updateOwnPost->description = 'Update own post';
	$updateOwnPost->ruleName = $rule->name;
	$auth->add($updateOwnPost);
	
	// "updateOwnPost" 权限将由 "updatePost" 权限使用
	$auth->addChild($updateOwnPost, $updatePost);
	
	// 允许 "author" 更新自己的帖子
	$auth->addChild($author, $updateOwnPost);
	
	
验证的代码：

	if (\Yii::$app->user->can('updatePost', ['post' => $post])) {
	    // 更新帖子
	}
	
这里的post是上文execute方法传的参数$params['post']；


验证流程：

![验证流程](http://www.yiichina.com/docs/guide/2.0/images/rbac-access-check-2.png) 

我们从图中的 updatePost 开始，经过 updateOwnPost。为通过检查，Authorrule 规则的 execute() 方法应当返回 true 。该方法从 can() 方法调用接收到 $params 参数， 因此它的值是 ['post' => $post] 。如果一切顺利，我们会达到指派给 John 的 author 角色。













