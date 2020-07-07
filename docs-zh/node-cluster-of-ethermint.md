### 一、Ethermint多节点搭建
> 前提
>
> 安装[ethermint](ethermint.md)

多节点集群由4个节点组成。
#### 生成四个节点配置
第一个节点：
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node1 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可以执行下方命令，正式环境不用加 emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node1
emintcli keys add node1

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node1 -a) 100000000000000000000000000photon,100000000000000000000000000stake

# 生成创建验证人的交易，gentx存储在~/.emintd/config/中
# 测试环境可选：emintd gentx --name node0 --keyring-backend test
emintd gentx --name node1

# 将生成的质押交易添加到创世文件
emintd collect-gentxs

# 验证有效性
emintd validate-genesis

# 获取本节点node-id
emintd tendermint show-node-id
```
第二个节点：
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node2 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可以执行下方命令，正式环境不用加：emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node1
emintcli keys add node2

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node2 -a) 100000000000000000000000000photon,100000000000000000000000000stake

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
第三个节点：
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node3 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可选，正式环境不用加：emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node2
emintcli keys add node3

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node3 -a) 100000000000000000000000000photon,100000000000000000000000000stake

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
第四个节点：
```
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
emintd init node4 --chain-id 2020

# 客户端设置参数
emintcli config chain-id 2020
emintcli config output json
emintcli config indent true
emintcli config trust-node true

# 测试环境可选：emintcli config keyring-backend test

# 创建一个钱包作为您的验证人帐户，钱包名为node3
emintcli keys add node4

# 将该钱包地址添加到genesis文件中的genesis.app_state.accounts数组中
# 注意: 此命令使您可以设置通证数量。确保此帐户有币，这是测试网络上唯一的质押通证
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
emintd add-genesis-account $(emintcli keys show node4 -a) 100000000000000000000000000photon,100000000000000000000000000stake

# 生成创建验证人的交易，gentx存储在~/.emintd/config/中
# 测试环境可选：emintd gentx --name node3 --keyring-backend test
emintd gentx --name node4

# 将生成的质押交易添加到创世文件
emintd collect-gentxs

# 验证有效性
emintd validate-genesis

# 获取本节点node-id
emintd tendermint show-node-id
```
#### 编辑`config.toml`文件
- 找到任意`nodeN/config`目录下的`config.toml`文件，将四个节点的node-id和ip替换成如下类似内容
```shell script
persistent_peers = "e0b95b151743c06dcc9c24eb9dd55000b55d6532@47.103.38.41:26656,d9b18e81ead40a8f132f8d3578a5888040041e79@101.133.151.154:26656,257160684f00b95e52e8c603ee0f4446eaa3ed10@101.133.225.179:26656,40c58f3a25f6037c12812a1daf4245f3a0c04993@47.103.97.77:26656"
```
- 把网络别名更改为自己设置的
```shell script
moniker = "node0"
```
#### 编辑`genesis.json`文件
> 注意
>
> `genesis.json`文件中的区块链启动时间`genesis_time`要保持一模一样
- genesis.bank.balances 添加其他三个地址块（把其他节点`genesis.json`文件中的balances集合下的地址属性复制粘贴到这里）
```shell script
"balances": [
        {
          "address": "cosmos14t886kkc2qqhd3hvgdxjdajfz9y3sp0vkzjlsc",
          "coins": [
            {
              "denom": "photon",
              "amount": "1000000000000000000"
            },
            {
              "denom": "stake",
              "amount": "1000000000000000000"
            }
          ]
        },
        {
          "address": "cosmos173yc0q76237u54hu3v3d5wytz04elsdj9rnlqh",
          "coins": [
            {
              "denom": "photon",
              "amount": "1000000000000000000"
            },
            {
              "denom": "stake",
              "amount": "1000000000000000000"
            }
          ]
        },
        {
          "address": "cosmos1ufly9xqlsdnhvehmn4jaml3tdv6lg06ek6w3k2",
          "coins": [
            {
              "denom": "photon",
              "amount": "100000000000000000000000000"
            },
            {
              "denom": "stake",
              "amount": "1000000000000000000"
            }
          ]
        },
        {
          "address": "cosmos1qxhgxzfxhx6ay7ysrv790x6zl2kndac6k9swr6",
          "coins": [
            {
              "denom": "photon",
              "amount": "100000000000000000000000000"
            },
            {
              "denom": "stake",
              "amount": "1000000000000000000"
            }
          ]
        }
      ]
```
- genesis.auth.accounts 添加其他三个初始账户信息属性（把其他节点`genesis.json`文件中的accounts初始账户信息集合中的账户属性复制粘贴到这里）
```shell script
"accounts": [
    {
      "type": "ethermint/EthAccount",
      "value": {
        "address": "cosmos14t886kkc2qqhd3hvgdxjdajfz9y3sp0vkzjlsc",
        "coins": [],
        "public_key": null,
        "account_number": 0,
        "sequence": 0,
        "code_hash": "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470"
      }
    },
    {
      "type": "ethermint/EthAccount",
      "value": {
        "address": "cosmos173yc0q76237u54hu3v3d5wytz04elsdj9rnlqh",
        "coins": [],
        "public_key": null,
        "account_number": 0,
        "sequence": 0,
        "code_hash": "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470"
      }
    },
    {
      "type": "ethermint/EthAccount",
      "value": {
        "address": "cosmos1ufly9xqlsdnhvehmn4jaml3tdv6lg06ek6w3k2",
        "coins": [],
        "public_key": null,
        "account_number": 0,
        "sequence": 0,
        "code_hash": "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470"
      }
    },
    {
      "type": "ethermint/EthAccount",
      "value": {
        "address": "cosmos1qxhgxzfxhx6ay7ysrv790x6zl2kndac6k9swr6",
        "coins": [],
        "public_key": null,
        "account_number": 0,
        "sequence": 0,
        "code_hash": "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470"
      }
    }
  ]
```
- genesis.genutil.gentxs 添加其他三个验证人属性（把其他节点`genesis.json`文件中的验证人属性复制粘贴到这里）
```shell script
"gentxs": [
        {
          "type": "cosmos-sdk/StdTx",
          "value": {
            "msg": [
              {
                "type": "cosmos-sdk/MsgCreateValidator",
                "value": {
                  "description": {
                    "moniker": "node1"
                  },
                  "commission": {
                    "rate": "0.100000000000000000",
                    "max_rate": "0.200000000000000000",
                    "max_change_rate": "0.010000000000000000"
                  },
                  "min_self_delegation": "1",
                  "delegator_address": "cosmos14t886kkc2qqhd3hvgdxjdajfz9y3sp0vkzjlsc",
                  "validator_address": "cosmosvaloper14t886kkc2qqhd3hvgdxjdajfz9y3sp0vnkx2ut",
                  "pubkey": "cosmosvalconspub1zcjduepq3gpf7h2y7ztmg93fezqk2u4pvxfs439uvnh8vq8j0v26v3yffpzsp5x47k",
                  "value": {
                    "denom": "stake",
                    "amount": "100000000"
                  }
                }
              }
            ],
            "fee": {
              "amount": [],
              "gas": "200000"
            },
            "signatures": [
              {
                "public_key": "8WWOMkEEKkNaeC8tOZ1j6dyejehsMUOLp8ntC/VVQrO9wB9X8CgCzc0NRFJlZrtVNATAbQx/NFouRZ19bvWIWnpPE1YpvQ==",
                "signature": "Zte3mrbCeAwpgGFPNKFbpr7YA6K/FAu2zx3wsMeBHLhLkMXsg9QtAimVxNGF/zoCfO72ffS9HEdpXZMy55BsBQA="
              }
            ],
            "memo": "e0b95b151743c06dcc9c24eb9dd55000b55d6532@172.19.166.128:26656"
          }
        },
        {
          "type": "cosmos-sdk/StdTx",
          "value": {
            "msg": [
              {
                "type": "cosmos-sdk/MsgCreateValidator",
                "value": {
                  "description": {
                    "moniker": "node2"
                  },
                  "commission": {
                    "rate": "0.100000000000000000",
                    "max_rate": "0.200000000000000000",
                    "max_change_rate": "0.010000000000000000"
                  },
                  "min_self_delegation": "1",
                  "delegator_address": "cosmos173yc0q76237u54hu3v3d5wytz04elsdj9rnlqh",
                  "validator_address": "cosmosvaloper173yc0q76237u54hu3v3d5wytz04elsdjqh82vy",
                  "pubkey": "cosmosvalconspub1zcjduepqkkr8t357c6ujuru2dgme35w3dcex9g6dt3yxllm4srjldh70tsgs74488y",
                  "value": {
                    "denom": "stake",
                    "amount": "100000000"
                  }
                }
              }
            ],
            "fee": {
              "amount": [],
              "gas": "200000"
            },
            "signatures": [
              {
                "public_key": "8WWOMkEE26mVS1BwZuQo8XWTVqU2FP2KzUzZNHGU5TfWU5t1E/h3ZNvpZHJ7MiH8FV3GCmAByqOknuvSYwVzsIcr7Tjzeg==",
                "signature": "gduUfYAii9a8p5X+POmgy6IIN3dl/EnuqFSCvm6AOihlr2JMZdw/WamV5mIB1kHW/I/TMQb9xIo3DSqvfVVRPgE="
              }
            ],
            "memo": "d9b18e81ead40a8f132f8d3578a5888040041e79@172.19.166.129:26656"
          }
        },
        {
          "type": "cosmos-sdk/StdTx",
          "value": {
            "msg": [
              {
                "type": "cosmos-sdk/MsgCreateValidator",
                "value": {
                  "description": {
                    "moniker": "node3"
                  },
                  "commission": {
                    "rate": "0.100000000000000000",
                    "max_rate": "0.200000000000000000",
                    "max_change_rate": "0.010000000000000000"
                  },
                  "min_self_delegation": "1",
                  "delegator_address": "cosmos1qxhgxzfxhx6ay7ysrv790x6zl2kndac6k9swr6",
                  "validator_address": "cosmosvaloper1qxhgxzfxhx6ay7ysrv790x6zl2kndac6n3ym0f",
                  "pubkey": "cosmosvalconspub1zcjduepqxegs58ykfghh2sdh9ltztel0fz77c400dx9ma9mdhyzl9wkehm2qhelapr",
                  "value": {
                    "denom": "stake",
                    "amount": "100000000"
                  }
                }
              }
            ],
            "fee": {
              "amount": [],
              "gas": "200000"
            },
            "signatures": [
              {
                "public_key": "8WWOMkEEcIyDNANQemnZl5fXk6ye3Y50/cdgc4PryAqxbueGwVfX65346z1kjm0RP5f4Gb1Dl39VqfZc1wjGw01PryBk+w==",
                "signature": "aSUXHyzG+91DvW4UsjtVU02ZksRdlbQPZ8NPIUUpPgdsCakOqOH6ddg8mJ/zFuvrZMlYWVzmY6A1xJGYRb6QOAE="
              }
            ],
            "memo": "257160684f00b95e52e8c603ee0f4446eaa3ed10@172.19.166.131:26656"
          }
        },
        {
          "type": "cosmos-sdk/StdTx",
          "value": {
            "msg": [
              {
                "type": "cosmos-sdk/MsgCreateValidator",
                "value": {
                  "description": {
                    "moniker": "node4"
                  },
                  "commission": {
                    "rate": "0.100000000000000000",
                    "max_rate": "0.200000000000000000",
                    "max_change_rate": "0.010000000000000000"
                  },
                  "min_self_delegation": "1",
                  "delegator_address": "cosmos1ufly9xqlsdnhvehmn4jaml3tdv6lg06ek6w3k2",
                  "validator_address": "cosmosvaloper1ufly9xqlsdnhvehmn4jaml3tdv6lg06enw6y6e",
                  "pubkey": "cosmosvalconspub1zcjduepqjcrwsmajkjg479cgsxll73ydk3q3xntr9m6um4lxzjnelrxnrrmqrhs70l",
                  "value": {
                    "denom": "stake",
                    "amount": "100000000"
                  }
                }
              }
            ],
            "fee": {
              "amount": [],
              "gas": "200000"
            },
            "signatures": [
              {
                "public_key": "8WWOMkEEA7tn349WI56HaH5qX31d0VYMblCqENCBC7B5ecSEeVILfPbaRB7iRZ5MLv9YuSjY1M0/yPCE8zOt7hly2FoVMA==",
                "signature": "DREVEUC7AFu4RRVTob28OX4LqbmoB8ElSvYakXUWGJ5RQ7RCg66e3rq9q63niUeCChHpaykRCftnnBSivm1GeQE="
              }
            ],
            "memo": "40c58f3a25f6037c12812a1daf4245f3a0c04993@172.19.166.127:26656"
          }
        }
      ]
```
#### 覆盖文件
覆盖之前的`config.toml`文件和`genesis.json`文件
>提示
>
>上一步`config.toml`文件编辑完之后，除了moniker不一致，其他内容一样，可以修改别名之后，将其分别覆盖四个节点的原来`config.toml`文件。
>上一步`genesis.json`文件编辑完之后，内容均一样，可以将其分别覆盖四个节点的原来`genesis.json`文件。

#### 后台启动
将四个节点分别以后台守护方式启动并保存日志
```shell script
nohup emintd start > init.log &
tail -f init.log
```

#### 启动Ethermint Web3 RPC API
接口访问可分为内网访问和公网访问，可以按照安全、性能自行设置
- 公网访问
```shell script
nohup emintcli rest-server --laddr "tcp://0.0.0.0:8545" > server.log &
tail -f server.log
```
- 内网访问
```shell script
nohup emintcli rest-server --laddr "tcp://localhost:8545" > server.log &
tail -f server.log
```

#### 导出ETH私钥
```shell script
emintcli keys unsafe-export-eth-key node1
```

### 二、运行全节点
```shell script
# 初始化节点
# 初始化genesis.json 文件：设置网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
# eg:emintd init <moniker> --chain-id 2020
emintd init node5 --chain-id 2020


# 下载多节点测试网络任意一个节点的 config.toml 和 genesis.json
# 替换当前`~/.emintd/config`目录下的`config.toml`文件和`genesis.json`文件

# 启动节点（也可使用 nohup 或 systemd 等方式后台运行）
emintd start
```
### 三、升级成为验证人节点
```shell script
# 创建一个新的密钥（钱包），或通过助记词/密钥库导入已有密钥。执行该命令后输入并确认密码，将生成一个新的密钥。密码至少8个字符。
# eg:emintcli keys add node5
emintcli keys add <key-name> <flags>
```
>注意
>
>在安全的地方备份好助记词！如果您忘记密码，这是恢复帐户的唯一方法。
>
- 从其他地方转入一些币到您刚刚创建的钱包中： 可能会用到以下命令
```
# 查询当前节点的钱包地址
emintcli keys list
# 查询钱包地址的余额
emintcli query account <account>
# 从其他地方转入一些币到您刚刚创建的钱包
emintcli tx send cosmos1lpa40757ka492ewy225qmmdcwr3gmdacmjm0yt cosmos1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd36tyk8 2000000stake --chain-id 2020 --fees=2stake
```
- 确认节点同步状态
```shell script
# 可以使用此命令安装 jq
# apt-get update && apt-get install -y jq

# 如果输出为 false, 则表明您的节点已经完成同步
emintcli status | jq .sync_info.catching_up
```
- 创建验证人

只有节点已完成同步时，才可以运行以下命令将您的节点升级为验证人：

下面命令中的moniker、chain_id、key_name设置为自己的
```shell script
emintcli tx staking create-validator \
  --amount=1000000stake \
  --pubkey=$(emintd tendermint show-validator) \
  --moniker="node5" \
  --chain-id=2020 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas-prices="0.025stake" \
  --from=node5
```
然后查询该交易hash结果查看是否成功成为验证人
```
emintcli query tx 6B913D5635953D7DC2D05DC03807C1433F00CBB1343EA6E0FBE3A8F403981961 | jq
```
输出命令
```shell script
{
  "height": "647",
  "txhash": "84DF3A0FC29FCD77814E86B1AB2B189540A921BEE7F44445E8330BBD5B858825",
  "raw_log": "[{\"msg_index\":0,\"log\":\"\",\"events\":[{\"type\":\"create_validator\",\"attributes\":[{\"key\":\"validator\",\"value\":\"cosmosvaloper1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd5wl365\"},{\"key\":\"amount\",\"value\":\"1000000\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"create_validator\"},{\"key\":\"module\",\"value\":\"staking\"},{\"key\":\"sender\",\"value\":\"cosmos1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd36tyk8\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "log": "",
      "events": [
        {
          "type": "create_validator",
          "attributes": [
            {
              "key": "validator",
              "value": "cosmosvaloper1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd5wl365"
            },
            {
              "key": "amount",
              "value": "1000000"
            }
          ]
        },
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "create_validator"
            },
            {
              "key": "module",
              "value": "staking"
            },
            {
              "key": "sender",
              "value": "cosmos1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd36tyk8"
            }
          ]
        }
      ]
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "159810",
  "tx": {
    "type": "cosmos-sdk/StdTx",
    "value": {
      "msg": [
        {
          "type": "cosmos-sdk/MsgCreateValidator",
          "value": {
            "description": {
              "moniker": "node5"
            },
            "commission": {
              "rate": "0.100000000000000000",
              "max_rate": "0.200000000000000000",
              "max_change_rate": "0.010000000000000000"
            },
            "min_self_delegation": "1",
            "delegator_address": "cosmos1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd36tyk8",
            "validator_address": "cosmosvaloper1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd5wl365",
            "pubkey": "cosmosvalconspub1zcjduepqljd7ct3rjqdrxf8qslplp97ja3jjn74sg0jdlyw5xp5er4mg6rlqdsq05q",
            "value": {
              "denom": "stake",
              "amount": "1000000"
            }
          }
        }
      ],
      "fee": {
        "amount": [
          {
            "denom": "stake",
            "amount": "5000"
          }
        ],
        "gas": "200000"
      },
      "signatures": [
        {
          "public_key": "8WWOMkEE4a2aLHkFXkQW+RHLfTPOTDIgGSTin4JjtCB3j5GjaxNTQZ55YK6uWwyoViOEHTaLUcpxZsjNlx/3ujAAG09/ag==",
          "signature": "/x73bRJi1Dvg1/tKnD4EL4h1OcVsPuOPUhJzSnL10SQ0QSPNMtyeawyb52WSEiPdZMMXQkeiSnuWJtfg0dBV8wE="
        }
      ],
      "memo": ""
    }
  },
  "timestamp": "2020-07-03T10:12:23Z"
}

I[2020-07-03|18:12:29.026] Executed block                               module=state height=647 validTxs=1 invalidTxs=0
I[2020-07-03|18:12:29.027] Updates to validators                        module=state updates=29BEC18C3707C1C0F91232D03AD04796DC3A62BC:1
I[2020-07-03|18:12:29.029] Committed state                              module=state height=647 txs=1 appHash=CAFD84C9B28E3A8FCA62A920E8E9223AE1CBB044D421697889CCD461D133EBEB
```
#### 查看所有验证人
```shell script
emintcli query staking validators
```
输出以下内容
```shell script
[
  {
    "operator_address": "cosmosvaloper1zmgajemeh2dh9vktyq43gslv6h86rg60dmrsrm",
    "consensus_pubkey": "cosmosvalconspub1zcjduepqkw43zu0jqnrthssya68zag3xpr48pjpm8gnn03xkzwtetrd6jvnssh76yt",
    "status": 3,
    "tokens": "100000000",
    "delegator_shares": "100000000.000000000000000000",
    "description": {
      "moniker": "node3"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "0.200000000000000000",
        "max_change_rate": "0.010000000000000000"
      },
      "update_time": "2020-07-03T02:03:43.859900471Z"
    },
    "min_self_delegation": "1"
  },
  {
    "operator_address": "cosmosvaloper1rsrf0fhmkkhnzdeetm4qwpl2sqh0cqtg7h45st",
    "consensus_pubkey": "cosmosvalconspub1zcjduepqyuwzq934vzypfeat2cm4ngtvymyzf05ucmeyts2z96tu36srl4ys7tjx4u",
    "status": 3,
    "tokens": "100000000",
    "delegator_shares": "100000000.000000000000000000",
    "description": {
      "moniker": "node4"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "0.200000000000000000",
        "max_change_rate": "0.010000000000000000"
      },
      "update_time": "2020-07-03T02:03:43.859900471Z"
    },
    "min_self_delegation": "1"
  },
  {
    "operator_address": "cosmosvaloper1hwp0fss8ngvfrjtsdhxhfrh96xtmclvd5wl365",
    "consensus_pubkey": "cosmosvalconspub1zcjduepqljd7ct3rjqdrxf8qslplp97ja3jjn74sg0jdlyw5xp5er4mg6rlqdsq05q",
    "status": 3,
    "tokens": "1000000",
    "delegator_shares": "1000000.000000000000000000",
    "description": {
      "moniker": "node5"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "0.200000000000000000",
        "max_change_rate": "0.010000000000000000"
      },
      "update_time": "2020-07-03T10:12:23.608925601Z"
    },
    "min_self_delegation": "1"
  },
  {
    "operator_address": "cosmosvaloper1h4vv50f7aw0kjvtzacfy378wskttlw0s5ryps8",
    "consensus_pubkey": "cosmosvalconspub1zcjduepqfj3ucjf8lqxmfm7vdnheyr0ccrkuva0fedvq5swa7l629trq93hqm3q5ds",
    "status": 3,
    "tokens": "100000000",
    "delegator_shares": "100000000.000000000000000000",
    "description": {
      "moniker": "node1"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "0.200000000000000000",
        "max_change_rate": "0.010000000000000000"
      },
      "update_time": "2020-07-03T02:03:43.859900471Z"
    },
    "min_self_delegation": "1"
  },
  {
    "operator_address": "cosmosvaloper1lpa40757ka492ewy225qmmdcwr3gmdac7x06gc",
    "consensus_pubkey": "cosmosvalconspub1zcjduepqnnk0gkxpfvdhfg3kunckj7adc46sruak55tm34kgqa72cheyfcqqm248m4",
    "status": 3,
    "tokens": "100000000",
    "delegator_shares": "100000000.000000000000000000",
    "description": {
      "moniker": "node2"
    },
    "unbonding_time": "1970-01-01T00:00:00Z",
    "commission": {
      "commission_rates": {
        "rate": "0.100000000000000000",
        "max_rate": "0.200000000000000000",
        "max_change_rate": "0.010000000000000000"
      },
      "update_time": "2020-07-03T02:03:43.859900471Z"
    },
    "min_self_delegation": "1"
  }
]
```
### 常用命令
[Ethermint常用命令使用指南](./emintcli-cmd.md )

### 常见错误
1. 提示`auth failure: secret conn failed`
> [2020-06-29|16:58:17.712] dialing failed (attempts: 5): auth failure: secret conn failed: EOF module=pex addr=40c58f3a25f6037c12812a1daf4245f3a0c04993@47.103.97.77:26656
>
> [2020-06-29|16:58:24.805] Stopping peer for error                      module=p2p peer="Peer{MConn{101.133.225.179:26656} 257160684f00b95e52e8c603ee0f4446eaa3ed10 out}" err=EOF

可能是`genesis.auth.accounts.code_hash`错误或者没有参数

![提示`auth failure: secret conn failed`](./images/ncoe-2.png) 

2. 提示`failed to execute message; message index: 0: failed to delegate; 0stake is smaller than 100000000stake: insufficient funds`

可能是`genesis.bank.balances`没有对应地址片段

![提示`failed to delegate`](./images/ncoe-3.png) 
