
### 委托一定数量的token给验证人
从自己的钱包中委托一定数量的token给验证人
```shell script
gaiacli tx staking delegate cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc 1000stake --from bhp
```
示例输出：
```shell script
{
    "chain_id":"chain-Yzniwz",
    "account_number":"10",
    "sequence":"17",
    "fee":{
        "amount":[

        ],
        "gas":"200000"
    },
    "msgs":[
        {
            "type":"cosmos-sdk/MsgDelegate",
            "value":{
                "delegator_address":"cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut",
                "validator_address":"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc",
                "amount":{
                    "denom":"stake",
                    "amount":"1000"
                }
            }
        }
    ],
    "memo":""
}
confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'bhp':
{
    "height":"0",
    "txhash":"EFDB437F882F90FF42EC8A813D18FEE66219C9EC2E2E4C482E8A737B129A7F7A",
    "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"action","value":"delegate"}]}]}]",
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
                            "value":"delegate"
                        }
                    ]
                }
            ]
        }
    ]
}
root@bhp-cosmos2:~# gaiacli query tx EFDB437F882F90FF42EC8A813D18FEE66219C9EC2E2E4C482E8A737B129A7F7A
{
  "height": "85762",
  "txhash": "EFDB437F882F90FF42EC8A813D18FEE66219C9EC2E2E4C482E8A737B129A7F7A",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"delegate\",\"attributes\":[{\"key\":\"validator\",\"value\":\"cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc\"},{\"key\":\"amount\",\"value\":\"1000\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"sender\",\"value\":\"cosmos1jv65s3grqf6v6jl3dp4t6c9t9rk99cd88lyufl\"},{\"key\":\"module\",\"value\":\"staking\"},{\"key\":\"sender\",\"value\":\"cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut\"},{\"key\":\"action\",\"value\":\"delegate\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut\"},{\"key\":\"amount\",\"value\":\"6025stake\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": [
        {
          "type": "delegate",
          "attributes": [
            {
              "key": "validator",
              "value": "cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc"
            },
            {
              "key": "amount",
              "value": "1000"
            }
          ]
        },
        {
          "type": "message",
          "attributes": [
            {
              "key": "sender",
              "value": "cosmos1jv65s3grqf6v6jl3dp4t6c9t9rk99cd88lyufl"
            },
            {
              "key": "module",
              "value": "staking"
            },
            {
              "key": "sender",
              "value": "cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut"
            },
            {
              "key": "action",
              "value": "delegate"
            }
          ]
        },
        {
          "type": "transfer",
          "attributes": [
            {
              "key": "recipient",
              "value": "cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut"
            },
            {
              "key": "amount",
              "value": "6025stake"
            }
          ]
        }
      ]
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "103190",
  "tx": {
    "type": "cosmos-sdk/StdTx",
    "value": {
      "msg": [
        {
          "type": "cosmos-sdk/MsgDelegate",
          "value": {
            "delegator_address": "cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut",
            "validator_address": "cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc",
            "amount": {
              "denom": "stake",
              "amount": "1000"
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
          "pub_key": {
            "type": "tendermint/PubKeySecp256k1",
            "value": "Am39sFUVa+cymPT4au9YiYvVBA76PSYGxmBsn7uUCARm"
          },
          "signature": "2Qs686RwVNx3Bz0GiZXAXee8oDszVQyjCEy7CPuKbhcIdKvTZdcarmnYVDc2rLlLXnm1Bjve+X4eKhUAW8dm9Q=="
        }
      ],
      "memo": ""
    }
  },
  "timestamp": "2020-06-22T10:12:35Z",
  "events": [
    {
      "type": "delegate",
      "attributes": [
        {
          "key": "validator",
          "value": "cosmosvaloper1v9sgfualu5uqdly0kvavjg4z5ppalfhkdlqrsc"
        },
        {
          "key": "amount",
          "value": "1000"
        }
      ]
    },
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "cosmos1jv65s3grqf6v6jl3dp4t6c9t9rk99cd88lyufl"
        },
        {
          "key": "module",
          "value": "staking"
        },
        {
          "key": "sender",
          "value": "cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut"
        },
        {
          "key": "action",
          "value": "delegate"
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut"
        },
        {
          "key": "amount",
          "value": "6025stake"
        }
      ]
    }
  ]
}
root@bhp-cosmos2:~# gaiacli query account cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut | jq
{
  "type": "cosmos-sdk/Account",
  "value": {
    "address": "cosmos1v9sgfualu5uqdly0kvavjg4z5ppalfhkgt5kut",
    "coins": [
      {
        "denom": "node1token",
        "amount": "100000"
      },
      {
        "denom": "stake",
        "amount": "18213"
      }
    ],
    "public_key": {
      "type": "tendermint/PubKeySecp256k1",
      "value": "Am39sFUVa+cymPT4au9YiYvVBA76PSYGxmBsn7uUCARm"
    },
    "account_number": "10",
    "sequence": "18"
  }
}

```
### 解绑一定数量的shares
从某个验证人解绑一定数量的shares
```shell script
 gaiacli tx staking unbond [validator-addr] [amount] [flags]
# 案例
gaiacli tx staking unbond cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj 100stake --from mykey
```

### 转委托
委托人可以将其抵押的token从一个验证人转移到另一个验证人。
- src-validator-addr：源验证者bech地址
- dst-validator-addr: 目标验证者bech地址
- mykey：自己的钱包名称
```shell script
gaiacli tx staking redelegate [src-validator-addr] [dst-validator-addr] [amount] [flags]
# 案例
gaiacli tx staking redelegate cosmosvaloper1gghjut3ccd8ay0zduzj64hwre2fxs9ldmqhffj cosmosvaloper1l2rsakp388kuv9k8qzq6lrm9taddae7fpx59wm 100stake --from mykey
```


