## Ethermint节点集群

### 常用命令
- 删除数据

此命令会删除守护程序`.emintd`和客户端`.emintcli`目录
```
rm -rf ~/.emint*
```
- 重置数据

可以使用此命令来重置节点，包括本地区块链数据库，地址簿文件，并将priv_validator.json重置为创世状态。

当本地区块链数据库以某种方式中断和无法同步或参与共识时，这是有用的。
```shell script
emintd unsafe-reset-all
```

### 多节点搭建

多节点集群由4个节点组成。

第一个节点
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node0 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可以执行下方命令，正式环境不用加
emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node0
emintcli keys add node0

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node0 -a) 100000000000000000000000000photon,1000000000000000000stake

# 生成创建验证人的交易，gentx存储在~/.emintd/config/中
# 测试环境可选：emintd gentx --name node0 --keyring-backend test
emintd gentx --name node0

# 将生成的质押交易添加到创世文件
emintd collect-gentxs

# 验证有效性
emintd validate-genesis

# 获取本节点node-id
emintd tendermint show-node-id
```
第二个节点
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node0 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可以执行下方命令，正式环境不用加
emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node1
emintcli keys add node1

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node1 -a) 1000000000000000000photon,1000000000000000000stake

# 生成创建验证人的交易，gentx存储在~/.emintd/config/中
# 测试环境可选：emintd gentx --name node1 --keyring-backend test
emintd gentx --name node1

# 将生成的质押交易添加到创世文件
emintd collect-gentxs

# 验证有效性
emintd validate-genesis

# 获取本节点node-id
emintd tendermint show-node-id
```
第三个节点
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node0 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可以执行下方命令，正式环境不用加
emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node2
emintcli keys add node2

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node2 -a) 1000000000000000000photon,1000000000000000000stake

# 生成创建验证人的交易，gentx存储在~/.emintd/config/中
# 测试环境可选：emintd gentx --name node2 --keyring-backend test
emintd gentx --name node2

# 将生成的质押交易添加到创世文件
emintd collect-gentxs

# 验证有效性
emintd validate-genesis

# 获取本节点node-id
emintd tendermint show-node-id
```
第四个节点
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node0 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可以执行下方命令，正式环境不用加
emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node3
emintcli keys add node3

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node3 -a) 1000000000000000000photon,1000000000000000000stake

# 生成创建验证人的交易，gentx存储在~/.emintd/config/中
# 测试环境可选：emintd gentx --name node3 --keyring-backend test
emintd gentx --name node3

# 将生成的质押交易添加到创世文件
emintd collect-gentxs

# 验证有效性
emintd validate-genesis

# 获取本节点node-id
emintd tendermint show-node-id
```




### 常见错误
1. 提示`auth failure: secret conn failed`
> [2020-06-29|16:58:17.712] dialing failed (attempts: 5): auth failure: secret conn failed: EOF module=pex addr=40c58f3a25f6037c12812a1daf4245f3a0c04993@47.103.97.77:26656
>
> [2020-06-29|16:58:24.805] Stopping peer for error                      module=p2p peer="Peer{MConn{101.133.225.179:26656} 257160684f00b95e52e8c603ee0f4446eaa3ed10 out}" err=EOF

可能是code_hash错误

![提示`auth failure: secret conn failed`](./images/ncoe-2.png) 

2. 提示`failed to execute message; message index: 0: failed to delegate; 0stake is smaller than 100000000stake: insufficient funds`

下图的中的balance没有添加对应地址段落

![提示`failed to delegate`](./images/ncoe-3.png) 
