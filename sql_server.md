##sql server使用记录

###重置登录密码
登录到之后执行：

	EXEC sp_password NULL, '你的新密码', 'sa'

或者使用windows账户登录，之后再执行