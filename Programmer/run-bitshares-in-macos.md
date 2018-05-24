###1.安装 Homebrew 

	/usr/bin/ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 
	
### 2.初始化 Homebrew 
	brew doctor 
	brew update 
手动关闭 brew 自动更新： 

	export HOMEBREW_NO_AUTO_UPDATE=true 
### 3.安装相关软件 
	brew install cmake git openssl autoconf automake berkeley-db google-perftools Doxygen libtool  
### 4.安装boost 
	brew install boost@1.57 
##### 注：
boost 安装时有版本要求，brew默认安装的都是最新版本，bitshares 安装boost 版本要求在 1.57-1.63 之间，过低过高都会导致 witness_node 的运行失败，也会导致 cli_wallet 失败(暂未试过)，boost 的安装路径需要在 cmake 命令行中明确指出，此处安装的是 1.57 版本 

### 5.克隆 bitshares 代码 
	git clone https://github.com/NebulaCybexDEX/bitshares-core.git  -- recursive 
### 6.修改 build 脚本
在 bitshares-core 根目录下，修改 build.sh 脚本中 boost 目录，保证路径正确 
![boots](https://github.com/NebulaCybexDEX/Document/blob/master/Programmer/bitshares-macos-1.png)

##### 注：
在运行脚本时会提示找不到 OpenSSL，这时你要明确的在 cmake 命令行中把 OpenSSL安装路径写出来
### 7. build
	在bitshares-core/build 目录下运行 build.sh 
### 8. bvt test （bvt: build validation test）
	运行 bvt.sh 
   
   测试是一个单节点的链。build 目录下有测试用的 genesis.json 与 config.ini 