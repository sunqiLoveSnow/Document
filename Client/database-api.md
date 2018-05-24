### 1.查询用户余额
查询用户余额有两种方式：一种是根据用户ID查询余额，第二种是根据用户名查询余额，两种方式返回的结果相同，均是资产ID及现有数量。

获取用户余额方法为：get\_account\_balances 或 get\_named\_account\_balances 示例如下：


	// 通过用户ID查询余额
	{
		“jsonrpc": "2.0",                         // json版本，固定为2.0
		"method": "get_account_balances",         // 获取余额的方法
		"params": ["1.2.25",["1.3.0"]],           // 参数1为用户ID，参数2为资产ID
		"id": 1                                   // 请求序列号
	}
	// 通过用户名查询余额
	{
		"jsonrpc": "2.0",   			          // json版本，固定为2.0
		"method": "get_named_account_balances",   // 获取余额的方法
		"params": ["nathan",["1.3.0"]],           // 参数1为用户名，参数2为资产ID
		"id": 2                                   // 请求序列号
	}
	
	// 返回结果
	{
		“id”:1,                                    // 对应请求序列号
		“jsonrpc":"2.0",
		“result”:[
			{
				“amount”:"463877816499",           // 资产数量
				“asset_id”:"1.3.0"                 // 资产ID
			}
		]
	}


#####注：
`资产数量是包含精度的`，如 CYB 资产精度为5，则上例中资产数量为4638778.16499

	// 获取资产ID及详情
	{
		"jsonrpc": "2.0", 
		"method": "list_assets",                   // 获取资产详情方法
		"params": ["AAA", 1],                      // 参数1为资产名，参数2为列出资产的个数，最大不能超 100
		"id": 3
	}
	
	// 返回结果
	{
		“id":3,
		“jsonrpc":"2.0",
		“result":[
		{
			“id”:"1.3.61",                          // 查询资产ID
			“symbol”:"AAA",                         // 资产符号
			“precision”:4,                          // 资产精度
			“issuer”:"1.2.159",                     // 资产发行人ID
			“options":{
				“max_supply”:1000000000,             // 最大供给量
				“market_fee_percent”:0,              // 市场手续费
				“max_market_fee”:0,                  // 最大市场手续费
				“issuer_permissions":79,
				“flags":0,
				“core_exchange_rate”:{               // 手续费汇率
					“base”:{                         // 基础资产数量
						“amount”:100000,             // 资产数量(含精度)
						“asset_id”:"1.3.0"},         // 资产ID
					“quote”:{                        // 报价资产数量
						“amount”:10000,              // 资产数量(含精度)
						“asset_id”:”1.3.61"}         // 资产ID
					},
				“whitelist_authorities”:[],          // 资产账户白名单
				“blacklist_authorities”:[],          // 资产账户黑名单
				“whitelist_markets":[],              // 资产市场白名单
				“blacklist_markets":[],              // 资产市场黑名单
				“description":"{
					”main”:”",                        // 描述
					“market":""}”,                    // 关注的市场交易对
				“extensions":[]
				},
			“dynamic_asset_data_id":"2.3.61"
			}
		]
	}
	
参数说明：

- core_exchange_rate: `核心汇率 = 报价资产数 /  基础资产数`    
核心汇率应高于市场价格，否则人们会从市场上购买你的令牌，并通过隐形套利来消耗你的资金池。核心汇率应定时更新，以反映资产的市场定价。
- issuer_permissions:  该值是由一系列权限或而来，包括是否收取交易手续费，是否要求资产持有人预先加入白名单，发行人是否可将资产收回，所有转账是否必须通过发行人审核同意，以及是否禁止隐私交易。定义了资产的可用特性，需要注意的是，即使发行人允许了某个特征，仍需要相应的标志来激活。创建之后，您只能删除给定的权限，但不能启用在创建时禁止的权限。
- flags：用来激活 issuer_permissions 所允许的权限，该值由一系列所要激活权限或得来。
- whitelist_authorities：当资产账户白名单为空时，不对账户进行限制；当账户白名单不为空时，只有账户同时也在该资产所在账户白名单内时，才可进行买卖。
- blacklist_authorities：在资产黑名单的人，不能对该资产进行买卖操作。
- whitelist_markets: 可与资产进行交换的其他资产；若为空，则默认可与所有资产进行交换。
- blacklist_markets: 不能与资产进行交换的其他资产。


### 2.查询市场上的买单 / 卖单
在市场中，所有的买单均可转化成卖单，例如，用户 cybex-test 希望以 100 个 CYB 买入 50 个资产 AAA，等同于用户 cybex-test 要以 50 个 AAA 的价格卖出100 个 CYB。

获取买卖单方法为 get\_limit\_orders 具体示例如下：

	{
		"jsonrpc": "2.0",                                    
		"method": "get_limit_orders",                // 获取限价单方法
		"params": ["1.3.0","1.3.61",10],             // 参数1、2为均为资产ID，参数3为返回交易记录最大数
		"id": 4                                                   
	}
	
	// 返回结果
	{
		“id":4,
		“jsonrpc":"2.0",
		“result”:[
		{     // 买单：用户期望以100CYB买入50AAA
			“id":"1.7.229",                            // 订单ID
			“expiration":"2023-05-18T08:51:54",        // 订单过期时间
			“seller":"1.2.157",                        // 订单发起者ID
			“for_sale":10000000,                       // 卖出资产数额(含资产精度)
			“sell_price":{
				“base":{                               // 卖出资产
					“amount":10000000,                 // 卖出资产数额(含精度)
					“asset_id":"1.3.0"},               // 卖出资产 ID
				“quote":{                              // 期待买入资产
					“amount":500000,                   // 期待最少买入资产数额(含精度)
					“asset_id":"1.3.61"}               // 期待买入资产ID
				},
			“deferred_fee”:500                         // 挂单的手续费
		},
		{     // 卖单：用户期望以30CYB价格卖出10个AAA
			“id”:"1.7.228",
			“expiration”:"2023-05-18T08:37:32",
			“seller”:"1.2.157",
			“for_sale”:100000,
			“sell_price":{
				“base”:{
					“amount”:100000,
					“asset_id”:"1.3.61"},
				“quote”:{
					“amount”:3000000,
					“asset_id”:"1.3.0"}
				},
			“deferred_fee":500
			}
		]
	}
##### 注：
不同资产，精度可以不同，在转换资产数额时，要根据该资产的精度进行转换

### 3. 查询交易记录
交易记录查询显示的是一段时间内两资产撮合成功的记录

获取交易记录方法为 get\_trade\_history 具体示例如下：
	
	{
		"jsonrpc": "2.0", 
		"method": "get_trade_history",               // 获取交易记录方法
		"params": ["1.3.0","1.3.81","2018-05-18T12:00:00","2018-05-16T12:00:00",10], 
		                                             // 参数1、2均为资产ID，参数3为较近的交易时间，参数4为较早交易时间，时间格式为UNIX时间,参数5为最多返回记录个数
		"id": 5
	}
	
	// 返回结果
	{
		"id":5,
		"jsonrpc":"2.0",
		"result"[
		{
			"date":"2018-05-18T09:22:00",            // 交易时间
			"price":"6.00000000000000000",           // 交易价格
			"amount":"40.00000000000000000",         // 交易数量
			"value":"240.00000000000000000"          // 交易总价
		},{
			"date":"2018-05-18T09:19:50",
			"price":"5.00000000000000000",
			"amount":"7.00000000000000000",
			"value":"35.00000000000000000"
		}]
	}
	
### 4. 获取账户详细信息
获取账户的详细信息

获取账户详情方法为 get\_full\_accounts 具体示例如下：

	{
		"jsonrpc": "2.0", 
		"method": "get_full_accounts",                // 获取全节点方法
		"params": [["1.2.157"], true],                // 参数1为账户ID，参数2为布尔值，是否要订阅消息
		"id": 6
	}
	// 返回结果
	{
		"id":6,
		"jsonrpc":"2.0",
		"result":[[
		"1.2.157",                                      // 账户ID
		{
			"account":                                  // 账户详情
			{
				"id":"1.2.157",
				"membership_expiration_date":"1970-01-01T00:00:00",
				"registrar":"1.2.18",                    // 注册人
				"referrer":"1.2.18",                     // 推荐人
				"lifetime_referrer":"1.2.18",            // 终生会员推荐人
				"network_fee_percentage":2000,           // 网络收取手续费百分比
				"lifetime_referrer_fee_percentage":3000, // 推荐人收取手续费百分比
				"referrer_rewards_percentage":0,         // 注册人与合作推荐人分享手续费百分比
				"name":"hxy-dextest",                    // 账户名称
				"owner":                                 // 账户权限
				{
					"weight_threshold":1,                // 权限阈值 
					"account_auths":[],
					"key_auths":
					[["CYB5AXkKPuTAro9xV1PhevGDb8SUc91EdQmfxPpa6WuXSVkQqnMxN",1]],     // 账户权限公钥及权重
					"address_auths":[]
				},
				"active":                                 // 活跃权限
				{
					"weight_threshold":1,
					"account_auths":[],
					"key_auths":
					[["CYB6H7LSpTTyFo5AiWAebeE3TdsAf2KvNiC3tTPkWMkkLb1aVm3ad",1]],      // 活跃权限公钥及权重
					"address_auths":[]
				},
				"options":
				{
					"memo_key":"CYB6H7LSpTTyFo5AiWAebeE3TdsAf2KvNiC3tTPkWMkkLb1aVm3ad",
					"voting_account":"1.2.5",
					"num_witness":0,
					"num_committee":0,
					"votes":[],
					"extensions":[]
				},
				"statistics":"2.6.157",
				"whitelisting_accounts":[],          // 将该账户设置为白名单的账户列表
				"blacklisting_accounts":[],          // 将该账户设置为黑名单的账户列表
				"whitelisted_accounts":[],           // 账户白名单
				"blacklisted_accounts":[],           // 账户黑名单
				"owner_special_authority":[0,{}],
				"active_special_authority":[0,{}],
				"top_n_control_flags":0,
				"level":97
			},
			"statistics":
			{
				"id":"2.6.157",
				"owner":"1.2.157",
				"most_recent_op":"2.9.26259",
				"total_ops":12,
				"removed_ops":0,
				"total_core_in_orders":10000000,
				"lifetime_fees_paid":28000,
				"pending_fees":0,
				"pending_vested_fees":0
			},
			"registrar_name":"owner1",              // 注册人账户名
			"referrer_name":"owner1",               // 引荐人账户名
			"lifetime_referrer_name":"owner1",      // 终生引荐人账户名
			"votes":[],
			"balances":[                            // 账户资产信息
			{
				"id":"2.5.173",
				"owner":"1.2.157",                   // 资产所有者
				"asset_type":"1.3.0",                // 资产ID
				"balance":"19962446000"              // 资产余额(含精度)
			},
			{
				"id":"2.5.175",
				"owner":"1.2.157",
				"asset_type":"1.3.61",
				"balance":1900000
			},
			{
				"id":"2.5.176",
				"owner":"1.2.157",
				"asset_type":"1.3.81",
				"balance":970000
			}],
			"vesting_balances":[],                   // 冻结资产信息
			"limit_orders":[                         // 账户挂单信息
			{
				"id":"1.7.228",                       // 挂单ID
				"expiration":"2023-05-18T08:37:32",   // 挂单超时时间
				"seller":"1.2.157",                   // 挂单发起者
				"for_sale":100000,                    // 欲卖资产数量(含精度)
				"sell_price":{                        // 买卖资产详情
					"base":{                          // 欲卖资产信息
						"amount":100000,              // 欲卖资产数量(含精度)
						"asset_id":"1.3.61"           // 欲卖资产ID
					},
					"quote":{                          // 欲换回资产详情
						"amount":3000000,              // 期望最少换回资产数量(含精度)
						"asset_id":"1.3.0"             // 欲换回资产ID
					}
				},
				"deferred_fee":500                      // 挂单手续费
			},
			{
				"id":"1.7.229",
				"expiration":"2023-05-18T08:51:54",
				"seller":"1.2.157",
				"for_sale":10000000,
				"sell_price":
				{
					"base":
					{
						"amount":10000000,
						"asset_id":"1.3.0"
					},
					"quote":
					{
						"amount":500000,
						"asset_id":"1.3.61"
					}
				},
				"deferred_fee":500
			}],
			"call_orders":[],                // 抵押订单详情
			"settle_orders":[],              // 清算资产详情
			"proposals":[],                  // 该账户发起的提议
			"assets":["1.3.81"],             // 该账户发行资产列表
			"withdraws":[]
		}]]
	}
	
	
##### 参数说明
- owner: 账户权限设定了谁可以控制本账户
- active: 活跃权限用来设定拥有花费本账户资金权限的账户名或公钥
- options: 交易附带的备注信息是使用备注公钥加密后传输的；了解备注信息需要对应的私钥进行解密。

	




### 5. 获取账户历史操作
返回用户一段时间内的操作记录，操作顺序由近及远。

获取账户操作历史方法 get\_account\_history 具体示例如下:

	{
		"jsonrpc": "2.0", 
		"method": "get_account_history",                // 获取历史操作方法
		"params": ["1.2.157","1.11.0", 10, "1.11.0"],   // 参数1为账户ID，参数2为操作结束ID，参数3为返回最多操作个数，参数4为操作开始ID
		"id": 14
	}
	// 返回结果
	{
		"id":14,
		"jsonrpc":"2.0",
		"result":[
		{
			"id":"1.11.14040",                          // 操作序号，返回结果中最晚操作在最前面
			"op":[
				0,                                      // 操作代码：转账操作
				{
					"fee":{                             // 该操作手续费
						"amount":2000,       
						"asset_id":"1.3.0"
						},
					"from":"1.2.157",                  // 资产来源
					"to":"1.2.159",                    // 资产流向
					"amount":{                         // 资产数量
						"amount":10000,
						"asset_id":"1.3.81"
						},
					"extensions":[]
				}
			],
			"result":[0,{}],
			"block_num":644625,
			"trx_in_block":0,
			"op_in_trx":0,
			"virtual_op":26115
		},
		{
			"id":"1.11.13900",
			"op":[
				1,                                       // 创建挂单操作
				{
					"fee":{                              // 手续费
						"amount":500,
						"asset_id":"1.3.0"
						},
					"seller":"1.2.157",                  // 发起者
					"amount_to_sell":{                   // 欲卖出资产信息
						"amount":24000000,
						"asset_id":"1.3.0"
						},
					"min_to_receive":{                   // 期待最少换回资产信息
						"amount":400000,
						"asset_id":"1.3.81"
						},
					"expiration":"2023-05-18T09:21:56",  // 超时时间
					"fill_or_kill":false,
					"extensions":[]
				}
			],
			"result":[1,"1.7.233"],
			"block_num":604949,
			"trx_in_block":0,
			"op_in_trx":0,
			"virtual_op":25654
		}
	}
	
##### 备注：
- op 代码：
	
		0: transfer_operation,
        1: limit_order_create_operation,
        2: limit_order_cancel_operation,
        3: call_order_update_operation,
        4: fill_order_operation,           
        5: account_create_operation,
        6: account_update_operation,
        7: account_whitelist_operation,
        8: account_upgrade_operation,
        9: account_transfer_operation,
        10: asset_create_operation,
        11: asset_update_operation,
        12: asset_update_bitasset_operation,
        13: asset_update_feed_producers_operation,
        14: asset_issue_operation,
        15: asset_reserve_operation,
        16: asset_fund_fee_pool_operation,
        17: asset_settle_operation,
        18: asset_global_settle_operation,
        19: asset_publish_feed_operation,
        20: witness_create_operation,
        21: witness_update_operation,
        22: proposal_create_operation,
        23: proposal_update_operation,
        24: proposal_delete_operation,
        25: withdraw_permission_create_operation,
        26: withdraw_permission_update_operation,
        27: withdraw_permission_claim_operation,
        28: withdraw_permission_delete_operation,
        29: committee_member_create_operation,
        30: committee_member_update_operation,
        31: committee_member_update_global_parameters_operation,
        32: vesting_balance_create_operation,
        33: vesting_balance_withdraw_operation,
        34: worker_create_operation,
        35: custom_operation,
        36: assert_operation,
        37: balance_claim_operation,
        38: override_transfer_operation,
        39: transfer_to_blind_operation,
        40: blind_transfer_operation,
        41: transfer_from_blind_operation,
        42: asset_settle_cancel_operation,  
        43: asset_claim_fees_operation,
        44: fba_distribute_operation,        
        45: initiate_crowdfund_operation,
        46: participate_crowdfund_operation,
        47: withdraw_crowdfund_operation