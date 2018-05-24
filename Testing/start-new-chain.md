### 启动新链

setp1. 更新 nathan-wallet.json 钱包

	[source update-wallet.sh]
setp2. 将链上初始冻结资产转入 nathan 账户

	[source import-balance.sh <acount> <wallet> <key>] 

* 所需脚本

		config.sh                     // 环境基本配置
		update-wallet.sh              // 更新钱包
		import-balance.sh             // 将冻结资产导入nathan账户
		import-balance.py      **//import_key,import_balance //**
		wallet/nathan-wallet.json     // nathan钱包
		key/nathan.key                // nathan key 文件
