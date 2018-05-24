### 创建资产
普通资产（uia）创建时需要一笔手续费，创建以后可以直接发行资产；
二元资产是资产代码中带"."的资产，创建资产后需1：1抵押出来再进行买卖操作。

* 所需脚本

		create-asset.sh   // 创建普通资产 uia
		create-asset.py    
		create-mia.sh     // 创建二元资产 mia
		create-mia.py     
		init-transfer.sh  // 从nathan账户向wallet/目录下钱包轮流转账 
		init-limit-orders.sh   // 产生买卖单
		sell.sh                // 卖单
		sell.py

##### 注
创建资产直接调用 database api 接口参数比较复杂，可以通过 create-asset.sh / py 进行资产创建。

任何买单均可转化成卖单