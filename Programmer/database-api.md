### 1.查询用户 CYB 余额：
* api 说明

   ** Return balances of the account
   
		get_account_balances(account_id_type id, const flat_set<asset_id_type>& assets); 
		      id:       ID of the account to get balances for                                                     //账号ID 
		  assets:       ID of the assets to get balances of; if empty, get all assets account has a balance in    //资产ID                                                                               
	** Return balances of the account by name instead of an ID                                   

		get_named_account_balances(const std::string& name, const flat_set<asset_id_type>& assets); 
		    name:       name of the account to get balances for       //账号名称 
		  assets:       ID of the assets to get balances of           //资产ID 

* 示例：


	Request: 

		curl --data '{"jsonrpc": "2.0", "method": "get_account_balances", "params": ["1.2.25",["1.3.0"]], "id": 1}' http://IP:PORT/rpc 
		curl --data '{"jsonrpc": "2.0", "method": "get_named_account_balances", "params": ["nathan",["1.3.0"]], "id": 2}' http://IP:PORT/rpc 

	Response: 
	
		{  
		   "amount":"92375341202703”,                       
		   "asset_id":”1.3.0”                                            
		} 


### 2.查询市场上的买单 / 卖单：
* api 说明

	** Return the limit orders, ordered from least price to greatest 

		get_limit_orders(asset_id_type a, asset_id_type b, uint32_t limit) 
		     a:     ID of asset being sold                      //卖出资产 ID 
		     b:     ID of asset being purchased                 //买入资产 ID 
		 limit:     Maximum number of orders to retrieve        //返回交易记录最大数 

* 示例：

	Request: 
	
		curl  --data '{"jsonrpc": "2.0", "method": "get_limit_orders", "params": ["1.3.0","1.3.4",10], "id": 4}' http://IP:PORT/rpc 

	Response: 
	
		{  
		    "id":"1.7.109639”, 
		    "expiration":"2018-04-25T08:07:50”, 
		    "seller":"1.2.10317”, 
		    "for_sale":123642, 
		    "sell_price”: { 
		        "base": { 
		            "amount":123642, 
		            "asset_id":"1.3.0" 
		            }, 
		    "quote": { 
		        "amount":34400, 
		        "asset_id":"1.3.4" 
		        } 
		    }, 
		    "deferred_fee":55 
		} 


### 3.查询交易记录：
* api 说明

	** Return recent transactions in the market
	
		get_trade_history(const string& base, const string& quote, fc::time_point_sec start, fc::time_point_sec stop, unsigned limit = 100) 
		    base:    string name of the first asset                                       //资产名称 
		   quote:    string name of the second asset                                      //资产名称 
		   start:    start time as a UNIX timestamp, the latest trade to retrieve         //UNIX时间戳，最新交易”2018-04-25T00:00:00”格式 
		    stop:    stop time as a UNIX timestamp, the earliest trade to retrieve        //UNIX时间戳，最早交易  [stop, start) 
		   limit:    number of transactions to retrieve, capped at 100                    //返回交易记录个数，最多100条 

* 示例： 

	Request: 
			
		curl --data '{"jsonrpc": "2.0", "method": "get_trade_history", "params": ["1.3.0","1.3.4","2018-04-25T12:00:00","2018-04-24T12:00:00",10], "id": 3}' http://IP:PORT/rpc 

	Response: 
	
		{ 
		    "date":"2018-04-25T07:39:09”,                      
		    "price":"36.59243620033847577", 
		    "amount":"0.05967900000000000", 
		    "value":"2.18380000000000019" 
		} 