### 空投
空投就是根据 snapshot 查询账户余额（账户余额，冻结资产，工资[目前不支持] ）按照百分比，对其进行转账操作

step1. 钱包内使用 snapshot 命令生成快照

	[snapshot block 81980] ==> 2018-05-14.json

step2. 通过 snapshot 生成 4 个 output*.csv 文件

	[source list.sh 2018-05-14.json]
	output1.csv   // 账户余额  acount,amount
	output2.csv   // 冻结资产
	output3.csv   // 撤单资产
	output4.csv   // merge(output1, output3)
step3. 执行转账操作

	[source airdrop.sh <ratio>]
现脚本内只是针对账户余额和撤单资产(output4.csv) 和冻结资产(output2.csv) 进行空投操作

* 所需脚本
	
		airdrop.sh                     // 从某账户进行转账操作
		list-account-balance.py
		list-balance-objects.py
		list-orders.py
		list.sh                         // 根据快照生成账户列表及现有资金数额
		nathan.key
		merge.py
		transfer-to-account.py          // 根据账户名及现有资产，按一定比例<ratio>对其进行转账
		transfer-to-balance-object.py
		transfer-to-list.py
	
##### 注：
目前是从 nathan 账户往各账户内转账，转出账户需要签名，所以需要 nathan.key 文件