### 批量生产 ***-wallet.json 
proc1. 利用 empty-wallet.json 生成账户同名钱包

proc2. 更新钱包，使钱包内的 chain-id 与运行链 ID 一致 

proc3. 向钱包内导入秘钥

	[source create-wallet-json.sh]

* 所需脚本

		key/***.key                  // key所在目录
		wallet/                      // ***-wallet.json目标目录
		create-wallet-json.sh        // 批量创建json文件
		update-wallet.sh             // 更新钱包chain-id
		update-wallet.py
		import-key.sh                // 导入秘钥到钱包
		import-key.py

##### 注：
create-wallet-json.sh 和 update-wallet.sh 脚本内，kye/ 和 wallet/ 两目录根据具体目录名称不同需进行相应改变