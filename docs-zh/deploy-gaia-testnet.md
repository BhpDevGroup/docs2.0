## 部署私有的Gaia测试网
### 一、部署单节点
#### 前提
- [安装gaia](install-gaia.md )
#### 创建genesis.json文件并启动网络
```shell script
# You can run all of these commands from your home directory
cd $HOME

# Initialize the genesis.json file that will help you to bootstrap the network
gaiad init --chain-id=testing testing

# 创建一个钱包作为您的验证人帐户
gaiacli keys add validator

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
gaiad add-genesis-account $(gaiacli keys show validator -a) 1000000000stake,1000000000validatortoken

# 生成创建验证人的交易，gentx存储在~/.gaiad/config/中
gaiad gentx --name validator

# 将生成的质押交易添加到创世文件：Add the generated bonding transaction to the genesis file
gaiad collect-gentxs

# 现在可以启动gaiad了：Now its safe to start `gaiad`
gaiad start
```
#### gaiad unsafe-reset-all
可以使用此命令来重置节点，包括本地区块链数据库，地址簿文件，并将priv_validator.json重置为创世状态。

当本地区块链数据库以某种方式中断和无法同步或参与共识时，这是有用的。
```shell script
gaiad unsafe-reset-all
```
#### gaiad tendermint
- 查询可以在p2p连接中使用的唯一节点ID，例如在`config.toml`中seeds和persistent_peers的格式<node-id>@ip:26656。

节点ID存储在node_key.json中。
```shell script
gaiad tendermint show-node-id
```
- 查询Tendermint Pubkey，用于identify your validator,并将用于在共识过程中签署Pre-vote/Pre-commit。

Tendermint Key存储在priv_validator.json中，创建验证人后，请一定要记得备份。
```shell script
gaiad tendermint show-validator
```
- 查询bech32前缀验证人地址
```shell script
gaiad tendermint show-address
```
#### gaiad export
请参阅[导出区块状态](gaiad-export.md )。

### 二、部署多节点
#### 前提
- [安装gaia](install-gaia.md )
- [安装docker](install-docker.md )
- [安装docker-compose](install-docker-compose.md )

#### 建立
构建运行命令所需的gaiad二进制文件（linux）和tendermint/gaiadnodedocker映像localnet。该二进制文件将安装到容器中，并且可以进行更新以重建映像，因此您只需要构建映像一次。
```shell script
# 克隆gaia仓库
git clone https://github.com/cosmos/gaia.git

# 进入gais目录
cd gaia

# 构建二进制文件到./build
make build-linux

# 构建tendermint/gaiadnode镜像
make build-docker-gaiadnode
```
#### 启动
启动4个网络节点(此方式的钱包默认密码是`12345678`)
```shell script
make localnet-start
```
该命令使用gaiadnode映像创建4个节点网络。下表中是每个节点对应的端口：

|  节点编号   | P2P端口  | RPC端口  |
|  ----  | ----  | ----  |
| gaianode0  | 26656 | 26657 |
| gaianode1  | 26659 | 26660 |
| gaianode2  | 26661 | 26662 |
| gaianode3  | 26663 | 26664 |
- 如果更新了二进制文件版本，只需重新购建它并重新启动节点即可：
```shell script
make build-linux localnet-start
```
#### 停止
停止所有正在运行的节点：
```shell script
make testnet_stop
```

#### 结构
`make localnet-start`命令会调用`gaiad testnet`命令在 `./build` 目录下生成4个节点的测试网配置文件。
```shell script
$ tree -L 2 build/
build/
├── gaiacli
├── gaiad
├── gentxs
│   ├── node0.json
│   ├── node1.json
│   ├── node2.json
│   └── node3.json
├── node0
│   ├── gaiacli
│   │   ├── key_seed.json
│   │   └── keys
│   └── gaiad
│       ├── ${LOG:-gaiad.log}
│       ├── config
│       └── data
├── node1
│   ├── gaiacli
│   │   └── key_seed.json
│   └── gaiad
│       ├── ${LOG:-gaiad.log}
│       ├── config
│       └── data
├── node2
│   ├── gaiacli
│   │   └── key_seed.json
│   └── gaiad
│       ├── ${LOG:-gaiad.log}
│       ├── config
│       └── data
└── node3
    ├── gaiacli
    │   └── key_seed.json
    └── gaiad
        ├── ${LOG:-gaiad.log}
        ├── config
        └── data
```
#### 日志
日志保存在每个节点`./build/nodeN/gaiad/gaia.log`。您还可以直接通过Docker查看日志，例如：
```shell script
docker logs -f gaiadnode0
```
#### 密钥和账户
返回此密钥管理器存储的所有密钥的名称、类型、地址和公钥列表。
```shell script
gaiacli keys list --home ./build/node0/gaiacli
```

### 三、普通节点加入自己部署的测试网络
1. 获取上一步中四个验证人节点node-id和对应的P2P端口（先保存到临时文件）
```shell script
gaiad tendermint  show-node-id --home ./build/nodeN/gaiad
```
2. 下载`./build/nodeN/gaiad`目录中任意一个节点的`config.toml`和`genesis.json`到本地
> 主要目的是修改`config.toml`和`genesis.json`以供普通节点使用
- 找到`config.toml`文件中的`persistent_peers`,按照自己测试网络的四个验证人节点node-id和对应的P2P端口修改成类似以下内容
```shell script
persistent_peers = "6b765eacccacac40a5e2f63961da73cf1f5ee2a5@127.0.0.1:26656,3c928896e21580925c96f9833b4b63c8cc2dbd78@127.0.0.1:26659,5dee586a8b70ee18935cbb9f4aa2176bd31211e6@127.0.0.1:26661,9ab7b59fc5be3827576cbd4741dc546bff8695d3@127.0.0.1:26663"
```
- 找到`config.toml`文件中的`moniker`修改成类似以下内容
```shell script
# A custom human readable name for this node
moniker = "bhphub"
```
- 找到`genesis.json`文件中的`chain_id`（下一步初始化使用）
```shell script
  "chain_id": "chain-G7L8cJ",
```
3. 初始化节点
```shell script
gaiad init <your_custom_moniker> --chain-id=chain-G7L8cJ
gaiad start
```
4. 将步骤2中的`config.toml`和`genesis.json`替换步骤3中的对应文件，默认路径是`~/.gaiad/config/`目录下
5. 启动gaia节点
```shell script
gaiad start
```
>提示
>
>您可能会看到一些连接错误，这没关系，P2P网络正在尝试查找可用的连接
> 可以添加几个社区公开节点到config.toml中的persistent_peers。
### 四、普通节点升级为验证人节点
#### 创建钱包
- 创建一个新的密钥（钱包），或通过助记词/密钥库导入已有密钥。执行该命令后输入并确认密码，将生成一个新的密钥。密码至少8个字符。
```shell script
gaiacli keys add <key-name> <flags>
```
>注意
>
>在安全的地方备份好助记词！如果您忘记密码，这是恢复帐户的唯一方法。
- 从其他地方转入一些币到您刚刚创建的钱包中：
可能会用到以下命令
```shell script
# 查询当前节点的钱包地址
gaiacli keys list
# 查询钱包地址的余额
gaiacli query account <account>
# 从其他地方转入一些币到您刚刚创建的钱包
gaiacli tx send cosmos1ras3ts4pw6904rlhecjc2kqar0t26sputjgy9c cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut 900000stake --chain-id chain-Yzniwz --fees=2stake --home /home/gaia/build/node0/gaiacli/
```
#### 确认节点同步状态
```shell script
# 可以使用此命令安装 jq
# apt-get update && apt-get install -y jq

# 如果输出为 false, 则表明您的节点已经完成同步
gaiacli status | jq .sync_info.catching_up
```
gaiacli status | jq 输出命令如以下类似内容
```shell script
root@bhp-cosmos2:~# gaiacli status | jq
{
  "node_info": {
    "protocol_version": {
      "p2p": "7",
      "block": "10",
      "app": "0"
    },
    "id": "6bf94172dca55d256923eb0bc1340678f5a8efcd",
    "listen_addr": "tcp://0.0.0.0:26656",
    "network": "chain-Yzniwz",
    "version": "0.32.11",
    "channels": "4020212223303800",
    "moniker": "node0",
    "other": {
      "tx_index": "on",
      "rpc_address": "tcp://0.0.0.0:26657"
    }
  },
  "sync_info": {
    "latest_block_hash": "FFA1E0C3453B39274ED169E0C04F41CB4A342DD536B49D8D374A6F02C343BBB2",
    "latest_app_hash": "B995E5FE887A3A70170AF4AC46551F1E0F44B177457D120EF20CC9918C2C3EA0",
    "latest_block_height": "2659",
    "latest_block_time": "2020-06-17T07:27:48.950411877Z",
    "catching_up": false
  },
  "validator_info": {
    "address": "4D819781779C930FB6DF90A56518E649B3BA5FCE",
    "pub_key": {
      "type": "tendermint/PubKeyEd25519",
      "value": "iQ3vtfWZGFArk/bKzpN8uIqYrKLl6dS8YTGnQrlFDGM="
    },
    "voting_power": "100"
  }
}
```
#### 创建验证人
只有节点已完成同步时，才可以运行以下命令将您的节点升级为验证人：

下面命令中的moniker、chain_id、key_name设置为自己的

设置之前：
```shell script
gaiacli tx staking create-validator \
  --amount=1000000uatom \
  --pubkey=$(gaiad tendermint show-validator) \
  --moniker="choose a moniker" \
  --chain-id=<chain_id> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas="auto" \
  --gas-prices="0.025uatom" \
  --from=<key_name>
```

设置之后：
>注意
>
> --gas 和 --gas-prices 选择其中一种就行
```shell script
gaiacli tx staking create-validator \
  --amount=1000000stake \
  --pubkey=$(gaiad tendermint show-validator) \
  --moniker="bhphub" \
  --chain-id=chain-Yzniwz \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas-prices="0.025stake" \
  --from=bhp
```
输出以下结果执行正常
```shell script
{
    "height":"0",
    "txhash":"FE6B5714B99392ED15353C3F3A9165CAB4A92C075290F47F4D9D59D7B5464EC0",
    "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"action","value":"create_validator"}]}]}]",
    "logs":[
        {
            "msg_index":0,
            "success":true,
            "log":"",
            "events":[
                {
                    "type":"message",
                    "attributes":[
                        {
                            "key":"action",
                            "value":"create_validator"
                        }
                    ]
                }
            ]
        }
    ]
}
```
然后查询该交易hash结果查看是否成功成为验证人
```shell script
gaiacli query tx FE6B5714B99392ED15353C3F3A9165CAB4A92C075290F47F4D9D59D7B5464EC0 | jq
```
输出以下结果
```shell script

```
#### 如何备份验证人节点
安全备份验证人节点私钥非常重要，这是恢复验证人节点的唯一方法。请注意，这里指的是Tendermint密钥。
如果您使用的是软件签名（tendermint的默认签名方法），则您的Tendermint密钥位于<gaiad-home>/config/priv_validator.json中。最简单的方法是备份整个config文件夹。


>注意
>
>重要
>
>一定要备份好 home（默认为〜/.gaiad/）目录中的 config 目录！如果您的服务器磁盘损坏或您准备迁移服务器，这是恢复验证人的唯一方法。
### 扩展
- [Gaia命令行客户端部分命令使用指南](gaiacli.md )




