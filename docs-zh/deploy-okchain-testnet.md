## 部署私有的okchain测试网
### 一、部署单节点
#### 前提
- [安装okchain](install-okchain.md )
#### 创建genesis.json文件并启动网络
```shell script
# You can run all of these commands from your home directory
cd $HOME

# Initialize the genesis.json file that will help you to bootstrap the network
okchaind init --chain-id=testing testing

# 创建一个钱包作为您的验证人帐户
okchaincli keys add validator

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
okchaind add-genesis-account $(gaiacli keys show validator -a) 1000000000okt,1000000000tokt

# 生成创建验证人的交易，gentx存储在~/.gaiad/config/中
okchaind gentx --name validator

# 将生成的质押交易添加到创世文件：Add the generated bonding transaction to the genesis file
okchaind collect-gentxs

# 现在可以启动okchaind了：Now its safe to start `okchaind`
okchaind start
```