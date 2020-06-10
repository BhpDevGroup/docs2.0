# Gaia 部分命令使用指南

## gaiacli keys add
- 创建一个新的密钥（钱包），或通过助记词/密钥库导入已有密钥。
```shell script
gaiacli keys add <key-name> <flags>
```
- 帮助说明

gaiacli keys add --help
```shell script
Derive a new private key and encrypt to disk.
Optionally specify a BIP39 mnemonic, a BIP39 passphrase to further secure the mnemonic,
and a bip32 HD path to derive a specific account. The key will be stored under the given name
and encrypted with the given password. The only input that is required is the encryption password.

If run with -i, it will prompt the user for BIP44 path, BIP39 mnemonic, and passphrase.
The flag --recover allows one to recover a key from a seed passphrase.
If run with --dry-run, a key would be generated (or recovered) but not stored to the
local keystore.
Use the --pubkey flag to add arbitrary public keys to the keystore for constructing
multisig transactions.

You can add a multisig key by passing the list of key names you want the public
key to be composed of to the --multisig flag and the minimum number of signatures
required through --multisig-threshold. The keys are sorted by address, unless
the flag --nosort is set.

Usage:
  gaiacli keys add <name> [flags]

Flags:
      --account uint32            Account number for HD derivation
      --dry-run                   Perform action, but don't add key to local keystore
  -h, --help                      help for add
      --indent                    Add indent to JSON response
      --index uint32              Address index number for HD derivation
  -i, --interactive               Interactively prompt user for BIP39 passphrase and mnemonic
      --ledger                    Store a local reference to a private key on a Ledger device
      --multisig strings          Construct and store a multisig public key (implies --pubkey)
      --multisig-threshold uint   K out of N required signatures. For use in conjunction with --multisig (default 1)
      --no-backup                 Don't print out seed phrase (if others are watching the terminal)
      --nosort                    Keys passed to --multisig are taken in the order they're supplied
      --pubkey string             Parse a public key in bech32 format and save it to disk
      --recover                   Provide seed phrase to recover existing key instead of creating

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.gaiacli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```
flags:

|  名称   | 描述  |
|  ----  | ----  |
| -h, --help  | 查询命令帮助 |
| 单元格  | 单元格 |

###  创建密钥 
执行该命令后输入并确认密码，将生成一个新的密钥。密码至少8个字符。
>注意
>
>重要
>
>写下助记词并保存在安全的地方！如果你不慎忘记密码或丢失了密钥，这是恢复账户的唯一方法。
 

```shell script
root@bhp-cosmos2:/home/gaia# gaiacli keys add mykey
Enter a passphrase to encrypt your key to disk:
ERROR: password must be at least 8 characters
root@bhp-cosmos2:/home/gaia# gaiacli keys add mykey
Enter a passphrase to encrypt your key to disk:
Repeat the passphrase:

- name: mykey
  type: local
  address: cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4
  pubkey: cosmospub1addwnpepqf0fzt2xd0xv5jku6fphm3ehmed7qucdemhefxusexx50ez897kpcn9j2l6
  mnemonic: ""
  threshold: 0
  pubkeys: []


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

wide subject exact fortune pink cricket nose assume captain furnace tackle catch special nose reform toe garment happy toss share debris feed soda mom
```
### 通过助记词恢复密钥

## gaiacli keys list
返回此密钥管理器存储的所有密钥的名称、类型、地址和公钥列表。
- 列出所有密钥
```shell script
gaiacli keys list
```
- 操作示例
```shell script
root@bhp-cosmos2:~# gaiacli keys list --output json
[
    {
        "name":"mykey",
        "type":"local",
        "address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
        "pubkey":"cosmospub1addwnpepqf0fzt2xd0xv5jku6fphm3ehmed7qucdemhefxusexx50ez897kpcn9j2l6"
    },
    {
        "name":"node2",
        "type":"local",
        "address":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4",
        "pubkey":"cosmospub1addwnpepqv7jpyvpr5eppr07e39jmjm7whlxcygu8gxsr64ds3a4aqvhens9jppttan"
    }
]
```
## gaiacli query account
该命令用于查询特定地址的余额信息。
标识：

|  名称   | 类型 |默认 | 描述  |
|  ----  | ----  |----  |----  |
| -h, --help |        |       | 查询命令帮助 |
| --chain-id | string |       | tendermint节点的Chain ID |
| -h, --help |        |       | 查询命令帮助 |
| --trust-node | string | true | 是否验证节点响应的结果 |
```shell script
gaiacli query account <account_cosmos>
```
- 操作示例
```shell script
root@bhp-cosmos2:~#  gaiacli query account cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4 --chain-id=testing --output json
{
    "type":"cosmos-sdk/Account",
    "value":{
        "address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
        "coins":[
            {
                "denom":"stake",
                "amount":"900000000"
            },
            {
                "denom":"validatortoken",
                "amount":"1000000000"
            }
        ],
        "public_key":{
            "type":"tendermint/PubKeySecp256k1",
            "value":"Al6RLUZrzMpK3NJDfcc33lvgcw3O75SbkMmNR+RHL6wc"
        },
        "account_number":"0",
        "sequence":"1"
    }
}
```

## gaiacli tx send
- 发送令牌到另一个地址
```shell script
gaiacli tx send <sender_key_name_or_address> <recipient_address> 10faucetToken
  --chain-id=<chain_id>
```
- 操作示例
```shell script
root@bhp-cosmos2:~# gaiacli tx send cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4 cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4 10validatortoken --chain-id=testing
{
    "chain_id":"testing",
    "account_number":"0",
    "sequence":"3",
    "fee":{
        "amount":[

        ],
        "gas":"200000"
    },
    "msgs":[
        {
            "type":"cosmos-sdk/MsgSend",
            "value":{
                "from_address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
                "to_address":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4",
                "amount":[
                    {
                        "denom":"validatortoken",
                        "amount":"10"
                    }
                ]
            }
        }
    ],
    "memo":""
}
confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'mykey':
{
    "height":"0",
    "txhash":"797F3937A9B52ECB609E0B1B9B6337D150DD040F1034BFF4B8110CB1D14BF5F1",
    "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"action","value":"send"}]}]}]",
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
                            "value":"send"
                        }
                    ]
                }
            ]
        }
    ]
}
```
转账之后可以看到有一笔有效的交易
```shell script
I[2020-06-10|14:45:23.221] Executed block                               module=state height=1936 validTxs=1 invalidTxs=0
I[2020-06-10|14:45:23.224] Committed state                              module=state height=1936 txs=1 appHash=D9E8EEE9E29CBC53F9C39E8D1181A41AE6D3C7CC7BA2ACEF1611645510C4E851
```
## gaiacli query tx
按交易Hash查询交易
- 帮助说明
```shell script
root@bhp-cosmos2:~# gaiacli query tx -h
Query for a transaction by hash in a committed block

Usage:
  gaiacli query tx [hash] [flags]

Flags:
  -h, --help          help for tx
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.gaiacli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
- 实际操作
```shell script
root@bhp-cosmos2:~# gaiacli query tx 797F3937A9B52ECB609E0B1B9B6337D150DD040F1034BFF4B8110CB1D14BF5F1 --chain-id=testing
{
    "height":"1936",
    "txhash":"797F3937A9B52ECB609E0B1B9B6337D150DD040F1034BFF4B8110CB1D14BF5F1",
    "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"},{"key":"module","value":"bank"},{"key":"action","value":"send"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"},{"key":"amount","value":"10validatortoken"}]}]}]",
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
                            "key":"sender",
                            "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                        },
                        {
                            "key":"module",
                            "value":"bank"
                        },
                        {
                            "key":"action",
                            "value":"send"
                        }
                    ]
                },
                {
                    "type":"transfer",
                    "attributes":[
                        {
                            "key":"recipient",
                            "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                        },
                        {
                            "key":"amount",
                            "value":"10validatortoken"
                        }
                    ]
                }
            ]
        }
    ],
    "gas_wanted":"200000",
    "gas_used":"24805",
    "tx":{
        "type":"cosmos-sdk/StdTx",
        "value":{
            "msg":[
                {
                    "type":"cosmos-sdk/MsgSend",
                    "value":{
                        "from_address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
                        "to_address":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4",
                        "amount":[
                            {
                                "denom":"validatortoken",
                                "amount":"10"
                            }
                        ]
                    }
                }
            ],
            "fee":{
                "amount":[

                ],
                "gas":"200000"
            },
            "signatures":[
                {
                    "pub_key":{
                        "type":"tendermint/PubKeySecp256k1",
                        "value":"Al6RLUZrzMpK3NJDfcc33lvgcw3O75SbkMmNR+RHL6wc"
                    },
                    "signature":"vSng+kEQlnFnD+v+/GhnCatnxV4Kr6CLPVIG6eAZlSpHbQSpdds5/NjHot7Nwlpq/CYNQ0gYifbE212C+tM2Nw=="
                }
            ],
            "memo":""
        }
    },
    "timestamp":"2020-06-10T06:45:18Z",
    "events":[
        {
            "type":"message",
            "attributes":[
                {
                    "key":"sender",
                    "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                },
                {
                    "key":"module",
                    "value":"bank"
                },
                {
                    "key":"action",
                    "value":"send"
                }
            ]
        },
        {
            "type":"transfer",
            "attributes":[
                {
                    "key":"recipient",
                    "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                },
                {
                    "key":"amount",
                    "value":"10validatortoken"
                }
            ]
        }
    ]
}
```

## gaiacli query txs
- 搜索所有匹配指定条件的交易集合
```shell script
root@bhp-cosmos2:~# gaiacli query txs --events='message.sender=cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4' --chain-id testing
{
    "total_count":"3",
    "count":"3",
    "page_number":"1",
    "page_total":"1",
    "limit":"30",
    "txs":[
        {
            "height":"1798",
            "txhash":"43C6F368EA60CCD90660C4C755CFB07621CEDAA55C0723313CD2BB073B0B47C2",
            "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"},{"key":"module","value":"bank"},{"key":"action","value":"send"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"},{"key":"amount","value":"10validatortoken"}]}]}]",
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
                                    "key":"sender",
                                    "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                                },
                                {
                                    "key":"module",
                                    "value":"bank"
                                },
                                {
                                    "key":"action",
                                    "value":"send"
                                }
                            ]
                        },
                        {
                            "type":"transfer",
                            "attributes":[
                                {
                                    "key":"recipient",
                                    "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                                },
                                {
                                    "key":"amount",
                                    "value":"10validatortoken"
                                }
                            ]
                        }
                    ]
                }
            ],
            "gas_wanted":"200000",
            "gas_used":"27610",
            "tx":{
                "type":"cosmos-sdk/StdTx",
                "value":{
                    "msg":[
                        {
                            "type":"cosmos-sdk/MsgSend",
                            "value":{
                                "from_address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
                                "to_address":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4",
                                "amount":[
                                    {
                                        "denom":"validatortoken",
                                        "amount":"10"
                                    }
                                ]
                            }
                        }
                    ],
                    "fee":{
                        "amount":[

                        ],
                        "gas":"200000"
                    },
                    "signatures":[
                        {
                            "pub_key":{
                                "type":"tendermint/PubKeySecp256k1",
                                "value":"Al6RLUZrzMpK3NJDfcc33lvgcw3O75SbkMmNR+RHL6wc"
                            },
                            "signature":"+ZZPv8quckiGXpmRm7zHNk4XCKrPq8PZEgNOJkSsx+5GUjrczu/c1YCquHXY6AGkTxJenZhMGE/qv3NuKlN0ow=="
                        }
                    ],
                    "memo":""
                }
            },
            "timestamp":"2020-06-10T06:33:46Z",
            "events":[
                {
                    "type":"message",
                    "attributes":[
                        {
                            "key":"sender",
                            "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                        },
                        {
                            "key":"module",
                            "value":"bank"
                        },
                        {
                            "key":"action",
                            "value":"send"
                        }
                    ]
                },
                {
                    "type":"transfer",
                    "attributes":[
                        {
                            "key":"recipient",
                            "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                        },
                        {
                            "key":"amount",
                            "value":"10validatortoken"
                        }
                    ]
                }
            ]
        },
        {
            "height":"1872",
            "txhash":"75F64F41B59F58424D61F8DD9C368F469E1DA5A62846102D61842428F31EF3C9",
            "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"},{"key":"module","value":"bank"},{"key":"action","value":"send"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"},{"key":"amount","value":"10validatortoken"}]}]}]",
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
                                    "key":"sender",
                                    "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                                },
                                {
                                    "key":"module",
                                    "value":"bank"
                                },
                                {
                                    "key":"action",
                                    "value":"send"
                                }
                            ]
                        },
                        {
                            "type":"transfer",
                            "attributes":[
                                {
                                    "key":"recipient",
                                    "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                                },
                                {
                                    "key":"amount",
                                    "value":"10validatortoken"
                                }
                            ]
                        }
                    ]
                }
            ],
            "gas_wanted":"200000",
            "gas_used":"24805",
            "tx":{
                "type":"cosmos-sdk/StdTx",
                "value":{
                    "msg":[
                        {
                            "type":"cosmos-sdk/MsgSend",
                            "value":{
                                "from_address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
                                "to_address":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4",
                                "amount":[
                                    {
                                        "denom":"validatortoken",
                                        "amount":"10"
                                    }
                                ]
                            }
                        }
                    ],
                    "fee":{
                        "amount":[

                        ],
                        "gas":"200000"
                    },
                    "signatures":[
                        {
                            "pub_key":{
                                "type":"tendermint/PubKeySecp256k1",
                                "value":"Al6RLUZrzMpK3NJDfcc33lvgcw3O75SbkMmNR+RHL6wc"
                            },
                            "signature":"gUkDpRmNbcxq5KLwjF1gfbxXyYUHMtrlp38B7ePcpNVIEcDlqkjOkig5VUPoC1RlG9BAS+ZZxt/93FvxOViMVA=="
                        }
                    ],
                    "memo":""
                }
            },
            "timestamp":"2020-06-10T06:39:57Z",
            "events":[
                {
                    "type":"message",
                    "attributes":[
                        {
                            "key":"sender",
                            "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                        },
                        {
                            "key":"module",
                            "value":"bank"
                        },
                        {
                            "key":"action",
                            "value":"send"
                        }
                    ]
                },
                {
                    "type":"transfer",
                    "attributes":[
                        {
                            "key":"recipient",
                            "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                        },
                        {
                            "key":"amount",
                            "value":"10validatortoken"
                        }
                    ]
                }
            ]
        },
        {
            "height":"1936",
            "txhash":"797F3937A9B52ECB609E0B1B9B6337D150DD040F1034BFF4B8110CB1D14BF5F1",
            "raw_log":"[{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"},{"key":"module","value":"bank"},{"key":"action","value":"send"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"},{"key":"amount","value":"10validatortoken"}]}]}]",
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
                                    "key":"sender",
                                    "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                                },
                                {
                                    "key":"module",
                                    "value":"bank"
                                },
                                {
                                    "key":"action",
                                    "value":"send"
                                }
                            ]
                        },
                        {
                            "type":"transfer",
                            "attributes":[
                                {
                                    "key":"recipient",
                                    "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                                },
                                {
                                    "key":"amount",
                                    "value":"10validatortoken"
                                }
                            ]
                        }
                    ]
                }
            ],
            "gas_wanted":"200000",
            "gas_used":"24805",
            "tx":{
                "type":"cosmos-sdk/StdTx",
                "value":{
                    "msg":[
                        {
                            "type":"cosmos-sdk/MsgSend",
                            "value":{
                                "from_address":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4",
                                "to_address":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4",
                                "amount":[
                                    {
                                        "denom":"validatortoken",
                                        "amount":"10"
                                    }
                                ]
                            }
                        }
                    ],
                    "fee":{
                        "amount":[

                        ],
                        "gas":"200000"
                    },
                    "signatures":[
                        {
                            "pub_key":{
                                "type":"tendermint/PubKeySecp256k1",
                                "value":"Al6RLUZrzMpK3NJDfcc33lvgcw3O75SbkMmNR+RHL6wc"
                            },
                            "signature":"vSng+kEQlnFnD+v+/GhnCatnxV4Kr6CLPVIG6eAZlSpHbQSpdds5/NjHot7Nwlpq/CYNQ0gYifbE212C+tM2Nw=="
                        }
                    ],
                    "memo":""
                }
            },
            "timestamp":"2020-06-10T06:45:18Z",
            "events":[
                {
                    "type":"message",
                    "attributes":[
                        {
                            "key":"sender",
                            "value":"cosmos1scrgzt4tfdlp6wsw02mmmecnle5lse9s7zzhu4"
                        },
                        {
                            "key":"module",
                            "value":"bank"
                        },
                        {
                            "key":"action",
                            "value":"send"
                        }
                    ]
                },
                {
                    "type":"transfer",
                    "attributes":[
                        {
                            "key":"recipient",
                            "value":"cosmos12xcpedywk64xd9s4ngwt0eh24yywyde9m3vma4"
                        },
                        {
                            "key":"amount",
                            "value":"10validatortoken"
                        }
                    ]
                }
            ]
        }
    ]
}
```

## gaiacli query block
在给定高度获取区块的验证数据。如果未指定高度，则将使用最新高度作为默认高度。
- 按高度获取区块信息
```shell script
gaiacli query block 1936 --chain-id=testing
{
    "block_meta":{
        "block_id":{
            "hash":"704C20870FD48E4E41776E2516787C195E329DBD539EE400C15CA1D1CA73BAB5",
            "parts":{
                "total":"1",
                "hash":"C2738419B4E737B0209100B76994796AFBADB77179DF9D76C94FD2D896DBBAE0"
            }
        },
        "header":{
            "version":{
                "block":"10",
                "app":"0"
            },
            "chain_id":"testing",
            "height":"1936",
            "time":"2020-06-10T06:45:18.197151565Z",
            "num_txs":"1",
            "total_txs":"3",
            "last_block_id":{
                "hash":"5608907883D1357000F1D72B2A077EF96EFDD75E764880B68C3A0FC9F1CD2E3C",
                "parts":{
                    "total":"1",
                    "hash":"746E57853CE1E79C0111CA0B02557D4DA13B466DFE090756270F6A3D8ED1D579"
                }
            },
            "last_commit_hash":"AFD77AE2A36C26864C0388E8E48FF7E366DF8F511ECEE86BF92337A1244B79EA",
            "data_hash":"F42B9076F368084967B2BB7D67376D882A84D893B6E832F4966D90E107A611B3",
            "validators_hash":"C49EB6CA1C984549BA1884F64F0B144040597DDCA54659A6E9781F85A9AB8AD3",
            "next_validators_hash":"C49EB6CA1C984549BA1884F64F0B144040597DDCA54659A6E9781F85A9AB8AD3",
            "consensus_hash":"048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "app_hash":"FAF622011DB0629289EA71E816E5B05A97FC2FB14D53F14BE7E5CDD0CD78E066",
            "last_results_hash":"",
            "evidence_hash":"",
            "proposer_address":"8D88ABF3645F2DB2D78452A5DC17119FFEA09B0B"
        }
    },
    "block":{
        "header":{
            "version":{
                "block":"10",
                "app":"0"
            },
            "chain_id":"testing",
            "height":"1936",
            "time":"2020-06-10T06:45:18.197151565Z",
            "num_txs":"1",
            "total_txs":"3",
            "last_block_id":{
                "hash":"5608907883D1357000F1D72B2A077EF96EFDD75E764880B68C3A0FC9F1CD2E3C",
                "parts":{
                    "total":"1",
                    "hash":"746E57853CE1E79C0111CA0B02557D4DA13B466DFE090756270F6A3D8ED1D579"
                }
            },
            "last_commit_hash":"AFD77AE2A36C26864C0388E8E48FF7E366DF8F511ECEE86BF92337A1244B79EA",
            "data_hash":"F42B9076F368084967B2BB7D67376D882A84D893B6E832F4966D90E107A611B3",
            "validators_hash":"C49EB6CA1C984549BA1884F64F0B144040597DDCA54659A6E9781F85A9AB8AD3",
            "next_validators_hash":"C49EB6CA1C984549BA1884F64F0B144040597DDCA54659A6E9781F85A9AB8AD3",
            "consensus_hash":"048091BC7DDC283F77BFBF91D73C44DA58C3DF8A9CBC867405D8B7F3DAADA22F",
            "app_hash":"FAF622011DB0629289EA71E816E5B05A97FC2FB14D53F14BE7E5CDD0CD78E066",
            "last_results_hash":"",
            "evidence_hash":"",
            "proposer_address":"8D88ABF3645F2DB2D78452A5DC17119FFEA09B0B"
        },
        "data":{
            "txs":[
                "vgEoKBapCkaoo2GaChSGBoEuq0t+HToOere95xP+afhksBIUUbActI62qmaWFZoct+bqqQjiNyUaFAoOdmFsaWRhdG9ydG9rZW4SAjEwEgQQwJoMGmoKJuta6YchAl6RLUZrzMpK3NJDfcc33lvgcw3O75SbkMmNR+RHL6wcEkC9KeD6QRCWcWcP6/78aGcJq2fFXgqvoIs9Ugbp4BmVKkdtBKl12zn82Mei3s3CWmr8Jg1DSBiJ9sTbXYL60zY3"
            ]
        },
        "evidence":{
            "evidence":null
        },
        "last_commit":{
            "block_id":{
                "hash":"5608907883D1357000F1D72B2A077EF96EFDD75E764880B68C3A0FC9F1CD2E3C",
                "parts":{
                    "total":"1",
                    "hash":"746E57853CE1E79C0111CA0B02557D4DA13B466DFE090756270F6A3D8ED1D579"
                }
            },
            "precommits":[
                {
                    "type":2,
                    "height":"1935",
                    "round":"0",
                    "block_id":{
                        "hash":"5608907883D1357000F1D72B2A077EF96EFDD75E764880B68C3A0FC9F1CD2E3C",
                        "parts":{
                            "total":"1",
                            "hash":"746E57853CE1E79C0111CA0B02557D4DA13B466DFE090756270F6A3D8ED1D579"
                        }
                    },
                    "timestamp":"2020-06-10T06:45:18.197151565Z",
                    "validator_address":"8D88ABF3645F2DB2D78452A5DC17119FFEA09B0B",
                    "validator_index":"0",
                    "signature":"4okTl4PGAzAGgisd1oMTW0fizBfoY6MxuRfJ5A6PE7WXXFyMoonem3hlvb7b3KuwH6p0ttvIKinzKMqhwUjtBg=="
                }
            ]
        }
    }
}
```
## gaiacli query tendermint-validator-set
在指定高度获取全部验证人。如果未指定高度，则将使用最新高度作为默认高度。
- 帮助说明
```shell script
root@bhp-cosmos2:~# gaiacli query tendermint-validator-set -h
Get the full tendermint validator set at given height

Usage:
  gaiacli query tendermint-validator-set [height] [flags]

Flags:
  -h, --help          help for tendermint-validator-set
      --indent        indent JSON response
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.gaiacli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors

```
- 按高度获取验证人
```shell script
gaiacli query tendermint-validator-set 1936 --chain-id testing
{
    "block_height":"1936",
    "validators":[
        {
            "address":"cosmosvalcons13ky2humytukm94uy22jac9c3nll2pxct7syd5a",
            "pub_key":"cosmosvalconspub1zcjduepq82a8qry7aetaksage6xz3zu36r2gkfhqsa5stvzye4x4s42mvffs47yyur",
            "proposer_priority":"0",
            "voting_power":"100"
        }
    ]
}
```
- 获取最新的验证人
```shell script
gaiacli query tendermint-validator-set --chain-id testing
```

## gaiacli keys export

- 导出keystore
```shell script
gaiacli keys export <name> [flags]
```
- 帮助说明
```shell script
root@bhp-cosmos2:~/mytestnet# gaiacli keys export --help
Export a private key from the local keybase in ASCII-armored encrypted format.

Usage:
  gaiacli keys export <name> [flags]

Flags:
  -h, --help   help for export

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.gaiacli")
  -o, --output string     Output format (text|json) (default "text")
      --trace             print out full stack trace on errors
```
- 操作示例
```shell script
root@bhp-cosmos2:~/mytestnet# gaiacli keys export node2 --home /root/mytestnet/node2/gaiacli/ 
Enter passphrase to decrypt your key:
Enter passphrase to encrypt the exported key:
-----BEGIN TENDERMINT PRIVATE KEY-----
salt: ED6BE05D6539DADDB3254C17C4656D00
kdf: bcrypt

AS15MJLmGQNUqAgwo+eT51ObAkZ/pL5+Tpf2NWsrzcK3mEIbmOrxgbINl6KCMzb/
dLVKEd8wCXOq1bJmt/MrQbjU0qUFjJhF6JEu87Q=
=ZvRg
-----END TENDERMINT PRIVATE KEY-----
```

- 按交易Hash查询交易
