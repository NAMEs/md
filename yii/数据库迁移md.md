#YII 数据库迁移

##用处
1.用来生成数据库；
2.用来同步不同的工作人员的数据库；

##工具使用
所有的迁移都是通过yii migrate来进行操作的；

###创建迁移
	yii migrate/create <name>
	yii migrate/create create_news_table//创建表news
	//name参数只能有字母数字和下划线

以上的命令执行之后就会产生一个php类文件

	use yii\db\Schema;
	use yii\db\Migration;
	
	class m150101_185401_create_news_table extends \yii\db\Migration
	{
	    public function up()
	    {
	        $this->createTable('news', [
	            'id' => Schema::TYPE_PK,
	            'title' => Schema::TYPE_STRING . ' NOT NULL',
	            'content' => Schema::TYPE_TEXT,
	        ]);
	    }
	
	    public function down()
	    {
	        $this->dropTable('news');
	    }
	
	}
	
注意：并不是所有迁移都是可恢复的。例如，如果 up() 方法删除了表中的一行数据， 这将无法通过 down() 方法来恢复这条数据。有时候，你也许只是懒得去执行 down() 方法了， 因为它在恢复数据库迁移方面并不是那么的通用。在这种情况下， 你应当在 down() 方法中返回 false 来表明这个 migration 是无法恢复的。

###类型  
当你通过 migration 创建一张表或者字段的时候，你应该使用 抽象类型 而不是 实体类型， 这样一来你的迁移对象就可以从特定的 DBMS 当中抽离出来。 yii\db\Schema 类定义了一整套可用的抽象类型常量。这些常量的格式为 TYPE_<Name>。 例如，TYPE_PK 指代自增主键类型；TYPE_STRING 指代字符串类型。 当迁移对象被提交到某个特定的数据库的时候，这些抽象类型将会被转换成相对应的实体类型。 以 MySQL 为例，TYPE_PK 将会变成 int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY， 而 TYPE_STRING 则变成 varchar(255)。


例子：  

	/**
	 * Handles the creation for table `post`.
	 * Has foreign keys to the tables:
	 *
	 * - `user`
	 * - `category`
	 */
	class m160328_040430_create_post extends Migration
	{
	    /**
	     * @inheritdoc
	     */
	    public function up()
	    {
	        $this->createTable('post', [
	            'id' => $this->primaryKey(),
	            'author_id' => $this->integer()->notNull(),
	            'category_id' => $this->integer()->defaultValue(1),
	            'title' => $this->string(),
	            'body' => $this->text(),
	        ]);
	
	        // creates index for column `author_id`
	        $this->createIndex(
	            'idx-post-author_id',
	            'post',
	            'author_id'
	        );
	
	        // add foreign key for table `user`
	        $this->addForeignKey(
	            'fk-post-author_id',
	            'post',
	            'author_id',
	            'user',
	            'id',
	            'CASCADE'
	        );
	
	        // creates index for column `category_id`
	        $this->createIndex(
	            'idx-post-category_id',
	            'post',
	            'category_id'
	        );
	
	        // add foreign key for table `category`
	        $this->addForeignKey(
	            'fk-post-category_id',
	            'post',
	            'category_id',
	            'category',
	            'id',
	            'CASCADE'
	        );
	    }
	
	    /**
	     * @inheritdoc
	     */
	    public function down()
	    {
	        // drops foreign key for table `user`
	        $this->dropForeignKey(
	            'fk-post-author_id',
	            'post'
	        );
	
	        // drops index for column `author_id`
	        $this->dropIndex(
	            'idx-post-author_id',
	            'post'
	        );
	
	        // drops foreign key for table `category`
	        $this->dropForeignKey(
	            'fk-post-category_id',
	            'post'
	        );
	
	        // drops index for column `category_id`
	        $this->dropIndex(
	            'idx-post-category_id',
	            'post'
	        );
	
	        $this->dropTable('post');
	    }
	}
	
	
##添加行
	yii migrate/create add_position_to_post --fields="position:integer"

之后就会产生：  
  
	class m150811_220037_add_position_to_post extends Migration
	{
	    public function up()
	    {
	        $this->addColumn('post', 'position', $this->integer());//第一个参数是表，第二个是行
	    }
	
	    public function down()
	    {
	        $this->dropColumn('post', 'position');
	    }
	}

删除行则是和增加行是相反的
	yii migrate/create drop_position_from_post --fields="position:integer"

产生的代码：  

	class m150811_220037_drop_position_from_post extends Migration
	{
	    public function up()
	    {
	        $this->dropColumn('post', 'position');
	    }
	
	    public function down()
	    {
	        $this->addColumn('post', 'position', $this->integer());
	    }
	}

##添加关联表

	yii migrate/create create_junction_post_and_tag --fields="created_at:dateTime"

这时会产生的代码：  
  
	/**
	 * Handles the creation for table `post_tag`.
	 * Has foreign keys to the tables:
	 *
	 * - `post`
	 * - `tag`
	 */
	class m160328_041642_create_junction_post_and_tag extends Migration
	{
	    /**
	     * @inheritdoc
	     */
	    public function up()
	    {
	        $this->createTable('post_tag', [
	            'post_id' => $this->integer(),
	            'tag_id' => $this->integer(),
	            'created_at' => $this->dateTime(),
	            'PRIMARY KEY(post_id, tag_id)',
	        ]);
	
	        // creates index for column `post_id`
	        $this->createIndex(
	            'idx-post_tag-post_id',
	            'post_tag',
	            'post_id'
	        );
	
	        // add foreign key for table `post`
	        $this->addForeignKey(
	            'fk-post_tag-post_id',
	            'post_tag',
	            'post_id',
	            'post',
	            'id',
	            'CASCADE'
	        );
	
	        // creates index for column `tag_id`
	        $this->createIndex(
	            'idx-post_tag-tag_id',
	            'post_tag',
	            'tag_id'
	        );
	
	        // add foreign key for table `tag`
	        $this->addForeignKey(
	            'fk-post_tag-tag_id',
	            'post_tag',
	            'tag_id',
	            'tag',
	            'id',
	            'CASCADE'
	        );
	    }
	
	    /**
	     * @inheritdoc
	     */
	    public function down()
	    {
	        // drops foreign key for table `post`
	        $this->dropForeignKey(
	            'fk-post_tag-post_id',
	            'post_tag'
	        );
	
	        // drops index for column `post_id`
	        $this->dropIndex(
	            'idx-post_tag-post_id',
	            'post_tag'
	        );
	
	        // drops foreign key for table `tag`
	        $this->dropForeignKey(
	            'fk-post_tag-tag_id',
	            'post_tag'
	        );
	
	        // drops index for column `tag_id`
	        $this->dropIndex(
	            'idx-post_tag-tag_id',
	            'post_tag'
	        );
	
	        $this->dropTable('post_tag');
	    }
	}


##事务迁移  
实现事务迁移的一个更为简便的方法是把迁移的代码都放到 safeUp() 和 safeDown() 方法里面。 它们与 up() 和 down() 的不同点就在于它们是被隐式的封装到事务当中的。 如此一来，只要这些方法里面的任何一个操作失败了，那么所有之前的操作都会被自动的回滚。  
  
	use yii\db\Schema;
	use yii\db\Migration;
	
	class m150101_185401_create_news_table extends Migration
	{
	    public function safeUp()
	    {
	        $this->createTable('news', [
	            'id' => $this->primaryKey(),,
	            'title' => $this->string()->notNull(),
	            'content' => $this->text(),
	        ]);
	        
	        $this->insert('news', [
	            'title' => 'test 1',
	            'content' => 'content 1',
	        ]);
	    }
	
	    public function safeDown()
	    {
	        $this->delete('news', ['id' => 1]);
	        $this->dropTable('news');
	    }
	}

当你在 safeUp() 当中执行多个数据库操作的时候，你应该在 safeDown() 方法当中反转它们的执行顺序。


##访问数据库
迁移的基类 yii\db\Migration 提供了一整套访问和操作数据库的方法。 你可能会发现这些方法的命名和 yii\db\Command 类提供的 DAO 方法 很类似。 例如，yii\db\Migration::createTable() 方法可以创建一张新的表， 这和 yii\db\Command::createTable() 的功能是一模一样的。

使用 yii\db\Migration 所提供的方法的好处在于你不需要再显式的创建 yii\db\Command 实例， 而且在执行每个方法的时候都会显示一些有用的信息来告诉我们数据库操作是不是都已经完成， 还有它们完成这些操作花了多长时间等等。

如下是所有这些数据库访问方法的列表：

yii\db\Migration::execute(): 执行一条 SQL 语句
yii\db\Migration::insert(): 插入单行数据
yii\db\Migration::batchInsert(): 插入多行数据
yii\db\Migration::update(): 更新数据
yii\db\Migration::delete(): 删除数据
yii\db\Migration::createTable(): 创建表
yii\db\Migration::renameTable(): 重命名表名
yii\db\Migration::dropTable(): 删除一张表
yii\db\Migration::truncateTable(): 清空表中的所有数据
yii\db\Migration::addColumn(): 加一个字段
yii\db\Migration::renameColumn(): 重命名字段名称
yii\db\Migration::dropColumn(): 删除一个字段
yii\db\Migration::alterColumn(): 修改字段
yii\db\Migration::addPrimaryKey(): 添加一个主键
yii\db\Migration::dropPrimaryKey(): 删除一个主键
yii\db\Migration::addForeignKey(): 添加一个外键
yii\db\Migration::dropForeignKey(): 删除一个外键
yii\db\Migration::createIndex(): 创建一个索引
yii\db\Migration::dropIndex(): 删除一个索引
yii\db\Migration::addCommentOnColumn(): adding comment to column
yii\db\Migration::dropCommentFromColumn(): dropping comment from column
yii\db\Migration::addCommentOnTable(): adding comment to table
yii\db\Migration::dropCommentFromTable(): dropping comment from table


#提交迁移  

为了将数据库升级到最新的结构，你应该使用如下命令来提交所有新的迁移：  

	yii migrate  

这条命令会列出迄今为止所有未提交的迁移。如果你确定你需要提交这些迁移， 它将会*按照类名当中的时间戳的顺序*，一个接着一个的运行每个新的迁移类里面的 up() 或者是 safeUp() 方法。   
如果其中任意一个迁移提交**失败**了， 那么这条命令将会退出并停止剩下的那些还未执行的迁移。