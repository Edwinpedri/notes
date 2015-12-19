# Bitsahres Cookbook

## Install Bitshares 2.0

	## update and install system dependent
	apt update && apt upgrade
	add-apt-repository ppa:ubuntu-toolchain-r/test
	apt-get update && apt-get install gcc-4.9 g++-4.9 cmake make \
	                  libbz2-dev libdb++-dev libdb-dev \
	                  libssl-dev openssl libreadline-dev \
	                  autoconf libtool git
	
	# install boost
	BOOST_ROOT=/opt/boost_1_57_0
	sudo apt-get update
	sudo apt-get install autotools-dev build-essential \
	                     g++ libbz2-dev libicu-dev python-dev
	wget -c 'http://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.bz2/download'\
	     -O boost_1_57_0.tar.bz2
	sha256sum boost_1_57_0.tar.bz2
	# "910c8c022a33ccec7f088bd65d4f14b466588dda94ba2124e78b8c57db264967"
	tar xjf boost_1_57_0.tar.bz2
	cd boost_1_57_0/
	./bootstrap.sh "--prefix=$BOOST_ROOT"
	./b2 install
	
	# build bitshares
	BOOST_ROOT=/opt/boost_1_57_0
	CC=gcc-4.9 CXX=g++-4.9 cmake -DBOOST_ROOT="$BOOST_ROOT" DCMAKE_BUILD_TYPE=Release .
	make

## Update Bitshares 2.0

	git fetch
	git checkout NEW_VERSION
	git submodule update --init --recursive
	cmake .
	make

## Install Tools

Install `python-graphenelib` to use monitor deposit.

	apt install python3-setuptools python3-dev libxml2-dev libxslt-dev
	easy_install-3.4 autobahn
	easy_install-3.4 requests
	easy_install-3.4 ecdsa
	easy_install-3.4 crypto
	easy_install-3.4 pycrypto
	easy_install-3.4 scrypt
	
	git clone http://github.com/xeroc/python-graphenelib
	cd python-graphenelib
	
	python3 setup.py install --user


## Trust Node

start trusted node (full node) `port: 8090`.

	./programs/witness_node/witness_node --data-dir=trusted_node \
	                                     --rpc-endpoint="127.0.0.1:8090"

## Delayed Node

start delayed node (use for deposit) `port: 8091`.

	./programs/delayed_node/delayed_node --api-access permission.json \
	                                     --trusted-node="127.0.0.1:8090" \
	                                     --rpc-endpoint="127.0.0.1:8091" \
	                                     --data-dir delayed_node \
	                                     --seed-node "0.0.0.0:0" \
	                                     --seed-nodes "[]" \
	                                     --p2p-endpoint="0.0.0.0:0"

## CLI Wallet

start cli wallet (use for transfer and confirm deposit) `port: 8092`

	./programs/cli_wallet/cli_wallet --server-rpc-endpoint="ws://127.0.0.1:8090" \
	                                 --rpc-http-endpoint="192.168.250.8:8092"

## Command in Common Use

* `list_my_accounts` __查看当前钱包的所有账户__
* `list_account_balances ACCOUNT` __查看账户余额__
* `get_account_history ACCOUNT` __查看账户历史记录__
* `transfer F T AMOUNT ASSET "MEMO” true` __转账从 F 至 T 并广播到网络__
* `get_object` __获取网络中的对象__
* `info` __查看网络状况__
* `import_accounts JSON PASSWORD` __导入旧版本钱包__

## Create New Wallet
* use `cli_wallet` command and -w option