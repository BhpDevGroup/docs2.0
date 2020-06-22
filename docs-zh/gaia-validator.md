### 普通节点升级为验证人节点
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

### 编辑验证人
修改验证的的参数，包括佣金比率，验证人节点名称以及其他描述信息。

|  名称   | 类型  | 必须  | 默认  | 描述  |
|  ----  | ----  | ----  | ----  | ----  |
| --commission-rate  | float |  |  | 佣金比率 |
| --moniker  | string |  |  | 验证人名称 |
| --identity  | string  |  |  | 身份签名 |
| --website  | string |  |  | 网址 |
| --details  | string |  |  | 验证人节点详细信息 |

设置之前：
```shell script
gaiacli tx staking edit-validator \
  --moniker="choose a moniker" \
  --website="https://cosmos.network" \
  --identity=6A0D65E29A4CBC8E \
  --details="To infinity and beyond!" \
  --chain-id=<chain_id> \
  --gas="auto" \
  --gas-prices="0.025uatom" \
  --from=<key_name> \
  --commission-rate="0.10"
```
设置之后：
```shell script
gaiacli tx staking edit-validator \
  --moniker="bhphub" \
  --website="https://www.bhpa.io" \
  --details="To infinity and beyond!" \
  --chain-id=chain-Yzniwz \
  --gas-prices="0.025stake" \
  --from=bhp \
  --commission-rate="0.10"
```
返回结果：
```shell script
{
    "chain_id":"chain-Yzniwz",
    "account_number":"10",
    "sequence":"16",
    "fee":{
        "amount":[
            {
                "denom":"stake",
                "amount":"5000"
            }
        ],
        "gas":"200000"
    },
    "msgs":[
        {
            "type":"cosmos-sdk/MsgEditValidator",
            "value":{
                "Description":{
                    "moniker":"bhphub",
                    "identity":"[do-not-modify]",
                    "website":"https://www.bhpa.io",
                    "details":"To infinity and beyond!"
                },
                "address":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc",
                "commission_rate":"0.100000000000000000",
                "min_self_delegation":null
            }
        }
    ],
    "memo":""
}
confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'bhp':
{
    "height":"0",
    "txhash":"B860DC121A3D24229BE8C32812425EF6B476541A5A21C2E9EA5B46745980D563",
    "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"action","value":"edit_validator"}]}]}]",
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
                            "value":"edit_validator"
                        }
                    ]
                }
            ]
        }
    ]
}
root@bhp-cosmos2:~/.gaiad# gaiacli query tx B860DC121A3D24229BE8C32812425EF6B476541A5A21C2E9EA5B46745980D563
{
    "height":"81530",
    "txhash":"B860DC121A3D24229BE8C32812425EF6B476541A5A21C2E9EA5B46745980D563",
    "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"edit_validator","attributes":[{"key":"commission_rate","value":"rate: 0.100000000000000000, maxRate: 0.200000000000000000, maxChangeRate: 0.010000000000000000, updateTime: 2020-06-22 03:57:35.104336048 +0000 UTC"},{"key":"min_self_delegation","value":"1"}]},{"type":"message","attributes":[{"key":"module","value":"staking"},{"key":"sender","value":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc"},{"key":"action","value":"edit_validator"}]}]}]",
    "logs":[
        {
            "msg_index":0,
            "success":true,
            "log":"",
            "events":[
                {
                    "type":"edit_validator",
                    "attributes":[
                        {
                            "key":"commission_rate",
                            "value":"rate: 0.100000000000000000, maxRate: 0.200000000000000000, maxChangeRate: 0.010000000000000000, updateTime: 2020-06-22 03:57:35.104336048 +0000 UTC"
                        },
                        {
                            "key":"min_self_delegation",
                            "value":"1"
                        }
                    ]
                },
                {
                    "type":"message",
                    "attributes":[
                        {
                            "key":"module",
                            "value":"staking"
                        },
                        {
                            "key":"sender",
                            "value":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc"
                        },
                        {
                            "key":"action",
                            "value":"edit_validator"
                        }
                    ]
                }
            ]
        }
    ],
    "gas_wanted":"200000",
    "gas_used":"37191",
    "tx":{
        "type":"cosmos-sdk/StdTx",
        "value":{
            "msg":[
                {
                    "type":"cosmos-sdk/MsgEditValidator",
                    "value":{
                        "Description":{
                            "moniker":"bhphub",
                            "identity":"[do-not-modify]",
                            "website":"https://www.bhpa.io",
                            "details":"To infinity and beyond!"
                        },
                        "address":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc",
                        "commission_rate":"0.100000000000000000",
                        "min_self_delegation":null
                    }
                }
            ],
            "fee":{
                "amount":[
                    {
                        "denom":"stake",
                        "amount":"5000"
                    }
                ],
                "gas":"200000"
            },
            "signatures":[
                {
                    "pub_key":{
                        "type":"tendermint/PubKeySecp256k1",
                        "value":"Am39sFUVa+cymPT4au9YiYvVBA76PSYGxmBsn7uUCARm"
                    },
                    "signature":"bEpD3fSAZmR/ZRsxyI+ApUBZ+6SCEQdbth9RxIGA0uEARI8SCJt8AC2knPdkwe+7927kGMchrYCkOvL5XjRebw=="
                }
            ],
            "memo":""
        }
    },
    "timestamp":"2020-06-22T03:57:35Z",
    "events":[
        {
            "type":"edit_validator",
            "attributes":[
                {
                    "key":"commission_rate",
                    "value":"rate: 0.100000000000000000, maxRate: 0.200000000000000000, maxChangeRate: 0.010000000000000000, updateTime: 2020-06-22 03:57:35.104336048 +0000 UTC"
                },
                {
                    "key":"min_self_delegation",
                    "value":"1"
                }
            ]
        },
        {
            "type":"message",
            "attributes":[
                {
                    "key":"module",
                    "value":"staking"
                },
                {
                    "key":"sender",
                    "value":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc"
                },
                {
                    "key":"action",
                    "value":"edit_validator"
                }
            ]
        }
    ]
}
```
### 查询所有验证人
```shell script
 gaiacli query staking validators
```
输出结果
```shell script
[
    {
        "operator_address":"cosmosvaloper1ps5tawelalv5vmjjmqyenez45s45dm7nrudcxv",
        "consensus_pubkey":"cosmosvalconspub1zcjduepqf6kaq98x2kz4njrt5w24quz468e24pqevd0tk6ghzh2mhur0lexqvhgp4g",
        "jailed":false,
        "status":2,
        "tokens":"100000000",
        "delegator_shares":"100000000.000000000000000000",
        "description":{
            "moniker":"node3",
            "identity":"",
            "website":"",
            "details":""
        },
        "unbonding_height":"0",
        "unbonding_time":"1970-01-01T00:00:00Z",
        "commission":{
            "commission_rates":{
                "rate":"0.000000000000000000",
                "max_rate":"0.000000000000000000",
                "max_change_rate":"0.000000000000000000"
            },
            "update_time":"2020-06-17T03:32:13.227077009Z"
        },
        "min_self_delegation":"1"
    },
    {
        "operator_address":"cosmosvaloper1rcakdv9wwvug55yvkfx53hyug4n5m5sm7pe9yn",
        "consensus_pubkey":"cosmosvalconspub1zcjduepqzxc6xxmc906e0smc87gfh7a6wymqpvf0zc9e5vt2zemp5p5rhw8qxut6mq",
        "jailed":false,
        "status":2,
        "tokens":"100000000",
        "delegator_shares":"100000000.000000000000000000",
        "description":{
            "moniker":"node2",
            "identity":"",
            "website":"",
            "details":""
        },
        "unbonding_height":"0",
        "unbonding_time":"1970-01-01T00:00:00Z",
        "commission":{
            "commission_rates":{
                "rate":"0.000000000000000000",
                "max_rate":"0.000000000000000000",
                "max_change_rate":"0.000000000000000000"
            },
            "update_time":"2020-06-17T03:32:13.227077009Z"
        },
        "min_self_delegation":"1"
    },
    {
        "operator_address":"cosmosvaloper1ras3ts4pw6904rlhecjc2kqar0t26spuwxu3ft",
        "consensus_pubkey":"cosmosvalconspub1zcjduepq3yx7ld04nyv9q2un7m9vaymuhz9f3t9zuh5af0rpxxn59w29p33sgthczz",
        "jailed":false,
        "status":2,
        "tokens":"100000000",
        "delegator_shares":"100000000.000000000000000000",
        "description":{
            "moniker":"node0",
            "identity":"",
            "website":"",
            "details":""
        },
        "unbonding_height":"0",
        "unbonding_time":"1970-01-01T00:00:00Z",
        "commission":{
            "commission_rates":{
                "rate":"0.000000000000000000",
                "max_rate":"0.000000000000000000",
                "max_change_rate":"0.000000000000000000"
            },
            "update_time":"2020-06-17T03:32:13.227077009Z"
        },
        "min_self_delegation":"1"
    },
    {
        "operator_address":"cosmosvaloper1y0hk7xeqdqk50kmryp0e2xy4vklhyfvx0rpeyt",
        "consensus_pubkey":"cosmosvalconspub1zcjduepqua8r36armuzm3uhfk6lf6f8q0usmjd3wdfk8shz0ug2n63exkahsrc24sk",
        "jailed":false,
        "status":2,
        "tokens":"100000000",
        "delegator_shares":"100000000.000000000000000000",
        "description":{
            "moniker":"node1",
            "identity":"",
            "website":"",
            "details":""
        },
        "unbonding_height":"0",
        "unbonding_time":"1970-01-01T00:00:00Z",
        "commission":{
            "commission_rates":{
                "rate":"0.000000000000000000",
                "max_rate":"0.000000000000000000",
                "max_change_rate":"0.000000000000000000"
            },
            "update_time":"2020-06-17T03:32:13.227077009Z"
        },
        "min_self_delegation":"1"
    },
    {
        "operator_address":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc",
        "consensus_pubkey":"cosmosvalconspub1zcjduepq04sgtf5pgde6r47hrzh0r3sx4m0raxd86l5frczcxyjujpnjtmkst8s59w",
        "jailed":false,
        "status":2,
        "tokens":"1000000",
        "delegator_shares":"1000000.000000000000000000",
        "description":{
            "moniker":"bhphub",
            "identity":"",
            "website":"https://www.bhpa.io",
            "details":"To infinity and beyond!"
        },
        "unbonding_height":"0",
        "unbonding_time":"1970-01-01T00:00:00Z",
        "commission":{
            "commission_rates":{
                "rate":"0.100000000000000000",
                "max_rate":"0.200000000000000000",
                "max_change_rate":"0.010000000000000000"
            },
            "update_time":"2020-06-22T03:57:35.104336048Z"
        },
        "min_self_delegation":"1"
    }
]
```

### 查看验证人描述 
通过地址查询验证人
```shell script
gaiacli query staking validator cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc
```
输出结果
```shell script
{
    "operator_address":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc",
    "consensus_pubkey":"cosmosvalconspub1zcjduepq04sgtf5pgde6r47hrzh0r3sx4m0raxd86l5frczcxyjujpnjtmkst8s59w",
    "jailed":false,
    "status":2,
    "tokens":"1000000",
    "delegator_shares":"1000000.000000000000000000",
    "description":{
        "moniker":"bhphub",
        "identity":"",
        "website":"https://www.bhpa.io",
        "details":"To infinity and beyond!"
    },
    "unbonding_height":"0",
    "unbonding_time":"1970-01-01T00:00:00Z",
    "commission":{
        "commission_rates":{
            "rate":"0.100000000000000000",
            "max_rate":"0.200000000000000000",
            "max_change_rate":"0.010000000000000000"
        },
        "update_time":"2020-06-22T03:57:35.104336048Z"
    },
    "min_self_delegation":"1"
}
```
### 如何备份验证人节点
安全备份验证人节点私钥非常重要，这是恢复验证人节点的唯一方法。请注意，这里指的是Tendermint密钥。
如果您使用的是软件签名（tendermint的默认签名方法），则您的Tendermint密钥位于<gaiad-home>/config/priv_validator.json中。最简单的方法是备份整个config文件夹。


>注意
>
>重要
>
>一定要备份好 home（默认为〜/.gaiad/）目录中的 config 目录！如果您的服务器磁盘损坏或您准备迁移服务器，这是恢复验证人的唯一方法。