##计算公式

	VIP业绩 = VIP客户的可返利金额 +专属的可返利金额　；
	额外奖励 = 原本专属升为VIP的180天内的50% ；  

##数据库

###红酒窝数据库：
VIP用户信息及其下属用户信息  
成为VIP的时间  
  
##思路步骤
1. 用户下单，判断其身份；
2. 若为VIP，则记录到其自身的业绩表中，并记录到其之前所属的VIP（如果有的话）业绩表中，同时记录到相应的业绩明细表（两次）中；
3. 若为下属，则记录到其所属的VIP的业绩表和业绩明细表中；
