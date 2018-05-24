### 更新费率、参数
step1. 某账户发起提议

	[source update-fee/param.sh wallet/nathan-wallet.json update-fee/param.json]
setp2. 获取该提议的ID

	[source list_proposals.sh]
step3. 委员会投票

	[source approve-all.sh <proposal-id>]
step4. 查看结果 --> pending_parameters

	[python3 check-object.py 2.0.0]
* 所需脚本

		update-fee.json    // 更新费率：提议账户+新的费率
		update-fee.sh
		update-fee.py
		update-param.json  // 更新参数：提议账户+更新参数
		update-param.sh
		update-param.py
		list_proposals.sh  // 获取提议ID
		approve-all.sh     // 对该提议进行投票
		check-object.py    // 检查是否生效
		wallet/nathan-wallet.json // 提议账户钱包


##### 注：
更新费率或参数，是由某账户提议，然后委员会投票决定。list\_proposals.sh 获取的是提议的 ID，check\-object.py 查询的是 global\_properties（id: 2.0.0）,2.1.0 是 dynamic\_global\_properties。

global\_properties 显示的是全局参数，当存在提议时，会返回两组参数，一组为 parameters，即当前环境使用参数值，一组为 pennding parameters, 即下个维护期要使用的参数值。

dynamic\_global\_properties 这个维护期内一些参数值