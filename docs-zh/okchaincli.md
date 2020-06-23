# okchain 部分命令使用指南

## okchaincli keys add
- 创建一个新的密钥（钱包），或通过助记词/密钥库导入已有密钥。执行该命令后输入并确认密码，将生成一个新的密钥。密码至少8个字符。
>注意
>
>重要
>
>写下助记词并保存在安全的地方！如果你不慎忘记密码或丢失了密钥，这是恢复账户的唯一方法。
```shell script
okchaincli keys add <key-name> <flags>
```
- 帮助说明

```shell script
root@bhp-cosmos2:/home/gaia# gaiacli keys add --help
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
 
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# gaiacli keys add --help
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
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli keys add --help
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
  okchaincli keys add <name> [flags]

Flags:
      --account uint32            Account number for HD derivation
      --dry-run                   Perform action, but don't add key to local keystore
  -h, --help                      help for add
      --indent                    Add indent to JSON response
      --index uint32              Address index number for HD derivation
  -i, --interactive               Interactively prompt user for BIP39 passphrase and mnemonic
      --ledger                    Store a local reference to a private key on a Ledger device
  -m, --mnemonic string           Mnemonic words
      --multisig strings          Construct and store a multisig public key (implies --pubkey)
      --multisig-threshold uint   K out of N required signatures. For use in conjunction with --multisig (default 1)
      --no-backup                 Don't print out seed phrase (if others are watching the terminal)
      --nosort                    Keys passed to --multisig are taken in the order they're supplied
      --pubkey string             Parse a public key in bech32 format and save it to disk
      --recover                   Provide seed phrase to recover existing key instead of creating
  -y, --yes                       Overwrite the existing account without confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.okchaincli")
  -o, --output string     Output format (text|json) (default "text")
      --passwd string     Pass word of sender (default "12345678")
      --trace             print out full stack trace on errors
```

## okchaincli keys list
返回此密钥管理器存储的所有密钥的名称、类型、地址和公钥列表。
- 列出所有密钥
```shell script
okchiancli keys list
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli keys list
[
  {
    "name": "lpl",
    "type": "local",
    "address": "okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn",
    "pubkey": "okchainpub1addwnpepqw3uq924awnhkjw9lcm7t9l8hmkrmj9lwgak62zwe5gw3jl55a5eqx8hmtu"
  },
  {
    "name": "validator",
    "type": "local",
    "address": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm",
    "pubkey": "okchainpub1addwnpepqtku4frp65x5g6w3t40ejnmtvd3ysl4l4h9hujvz5cc9kv9hxzqax7lyuet"
  }
]root@iZwz9af3cg1abi4nmbogwxZ:~#
```
## okchaincli query account
该命令用于查询特定地址的余额信息。
标识：

|  名称   | 类型 |默认 | 描述  |
|  ----  | ----  |----  |----  |
| -h, --help |        |       | 查询命令帮助 |
| --chain-id | string |       | tendermint节点的Chain ID |
| -h, --help |        |       | 查询命令帮助 |
| --trust-node | string | true | 是否验证节点响应的结果 |
```shell script
okchaincli query account <account_cosmos>
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli query account okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm
{
  "type": "cosmos-sdk/Account",
  "value": {
    "address": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm",
    "coins": [
      {
        "denom": "okt",
        "amount": "1000000000.00000000"
      },
      {
        "denom": "tokt",
        "amount": "99999999.99900000"
      }
    ],
    "public_key": {
      "type": "tendermint/PubKeySecp256k1",
      "value": "Au3KpGHVDURp0V1fmU9rY2JIfr+ty35JgqYwWzC3MIHT"
    },
    "account_number": "1",
    "sequence": "1"
  }
}
```

## okchaincli tx send
- 发送令牌到另一个地址
```shell script
okchaincli tx send <sender_key_name_or_address> <recipient_address> 10faucetToken
  --chain-id=<chain_id>
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx send okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn 20okt --fees=0.01tokt
{
  "chain_id": "testing",
  "account_number": "1",
  "sequence": "1",
  "fee": {
    "amount": [
      {
        "denom": "tokt",
        "amount": "0.01000000"
      }
    ],
    "gas": "200000"
  },
  "msgs": [
    {
      "type": "okchain/token/MsgTransfer",
      "value": {
        "from_address": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm",
        "to_address": "okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn",
        "amount": [
          {
            "denom": "okt",
            "amount": "20.00000000"
          }
        ]
      }
    }
  ],
  "memo": ""
}

confirm transaction before signing and broadcasting [y/N]: y
{
  "height": "0",
  "txhash": "ECEA5CB94724CD120D8DA877B8C724988BDD328E56539E90EF9E1EEE214B3F24",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":null}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": null
    }
  ]
}
```
## okchaincli query tx
按交易Hash查询交易
- 帮助说明
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli query tx -h
Query for a transaction by hash in a committed block

Usage:
  okchaincli query tx [hash] [flags]

Flags:
  -h, --help          help for tx
  -n, --node string   Node to connect to (default "tcp://localhost:26657")
      --trust-node    Trust connected full node (don't verify proofs for responses)

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.okchaincli")
  -o, --output string     Output format (text|json) (default "text")
      --passwd string     Pass word of sender (default "12345678")
      --trace             print out full stack trace on errors
```
- 实际操作
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli query tx ECEA5CB94724CD120D8DA877B8C724988BDD328E56539E90EF9E1EEE214B3F24
{
  "height": "6029699",
  "txhash": "ECEA5CB94724CD120D8DA877B8C724988BDD328E56539E90EF9E1EEE214B3F24",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":null}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": null
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "44055",
  "tx": {
    "type": "cosmos-sdk/StdTx",
    "value": {
      "msg": [
        {
          "type": "okchain/token/MsgTransfer",
          "value": {
            "from_address": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm",
            "to_address": "okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn",
            "amount": [
              {
                "denom": "okt",
                "amount": "20.00000000"
              }
            ]
          }
        }
      ],
      "fee": {
        "amount": [
          {
            "denom": "tokt",
            "amount": "0.01000000"
          }
        ],
        "gas": "200000"
      },
      "signatures": [
        {
          "pub_key": {
            "type": "tendermint/PubKeySecp256k1",
            "value": "Au3KpGHVDURp0V1fmU9rY2JIfr+ty35JgqYwWzC3MIHT"
          },
          "signature": "yPREOR4XNxZkJvltUpKSzRxUyjY8CpuYfhs0s4322DIswe4dIY8y0wtwO6iNpYlXEFAxRamsRubyRbGwUxhJCA=="
        }
      ],
      "memo": ""
    }
  },
  "timestamp": "2020-06-22T06:12:26Z",
  "events": [
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "module",
          "value": "token"
        },
        {
          "key": "action",
          "value": "send"
        },
        {
          "key": "fee",
          "value": "0.01000000tokt"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn"
        },
        {
          "key": "amount",
          "value": "20.00000000okt"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "0.01000000tokt"
        }
      ]
    }
  ]
}
```
## okchaincli tx token issue
 - 发行一种代币
- 帮助说明

```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token issue -h
issue a token

Usage:
  okchaincli tx token issue [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --desc string             describe of the token
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 1tokt
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 1tokt)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for issue
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --mintable                whether the token can be minted
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --sequence uint           The sequence number of the signing account (offline mode only)
  -s, --symbol string           symbol of the new token
  -n, --total-supply string     total supply of the new token (default "0")
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -w, --whole-name string       whole name of the new token
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.okchaincli")
  -o, --output string     Output format (text|json) (default "text")
      --passwd string     Pass word of sender (default "12345678")
      --trace             print out full stack trace on errors
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token issue --from validator --symbol hcoin -n 200000 -w 'hcoin' --desc 'blockchain coin' --fees=0.01tokt -b block --mintable
{
  "chain_id": "testing",
  "account_number": "1",
  "sequence": "7",
  "fee": {
    "amount": [
      {
        "denom": "tokt",
        "amount": "0.01000000"
      }
    ],
    "gas": "200000"
  },
  "msgs": [
    {
      "type": "okchain/token/MsgIssue",
      "value": {
        "description": "blockchain coin",
        "symbol": "",
        "original_symbol": "hcoin",
        "whole_name": "hcoin",
        "total_supply": "200000",
        "owner": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm",
        "mintable": true
      }
    }
  ],
  "memo": ""
}

confirm transaction before signing and broadcasting [y/N]: y
{
  "height": "6036060",
  "txhash": "430487B48912E9511579C1C2154DE9D1EAD92C634C914D8EDED0DDD37B879774",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":null}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": null
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "100526",
  "events": [
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "okchain183rfa8tvtp6ax7jr7dfaf7ywv870sykx3uvcxn"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "module",
          "value": "token"
        },
        {
          "key": "fee",
          "value": "2500.01000000tokt"
        },
        {
          "key": "symbol",
          "value": "hcoin-977"
        },
        {
          "key": "action",
          "value": "issue"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "amount",
          "value": "200000.00000000hcoin-977"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "2500.00000000tokt"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "0.01000000tokt"
        }
      ]
    }
  ]
}
```
## okchaincli tx token mint
 - 增发一种代币（需要是代币的拥有者）
- 帮助说明

```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token mint -h
mint tokens for an existing token

Usage:
  okchaincli tx token mint [amount] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 1tokt
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 1tokt)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for mint
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.okchaincli")
  -o, --output string     Output format (text|json) (default "text")
      --passwd string     Pass word of sender (default "12345678")
      --trace             print out full stack trace on errors
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token mint 10000000bcoin-5b2 --fees=0.01tokt --from validator -b block
{
  "chain_id": "testing",
  "account_number": "1",
  "sequence": "5",
  "fee": {
    "amount": [
      {
        "denom": "tokt",
        "amount": "0.01000000"
      }
    ],
    "gas": "200000"
  },
  "msgs": [
    {
      "type": "okchain/token/MsgMint",
      "value": {
        "amount": {
          "denom": "bcoin-5b2",
          "amount": "10000000.00000000"
        },
        "owner": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
      }
    }
  ],
  "memo": ""
}

confirm transaction before signing and broadcasting [y/N]: y
{
  "height": "6030822",
  "txhash": "2097100ED89B4292BBC35EE3E64A97E2864F56590D55ADD923F893271A514283",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":null}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": null
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "83846",
  "events": [
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "okchain183rfa8tvtp6ax7jr7dfaf7ywv870sykx3uvcxn"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "module",
          "value": "token"
        },
        {
          "key": "fee",
          "value": "10.01000000tokt"
        },
        {
          "key": "action",
          "value": "mint"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "amount",
          "value": "10000000.00000000bcoin-5b2"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "10.00000000tokt"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "0.01000000tokt"
        }
      ]
    }
  ]
}
```
## okchaincli tx token burn
 - 销毁一定数量的代币（需要是代币的拥有者）
- 帮助说明

```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token burn -h
burn some amount of token

Usage:
  okchaincli tx token burn [amount] [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 1tokt
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 1tokt)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for burn
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --sequence uint           The sequence number of the signing account (offline mode only)
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.okchaincli")
  -o, --output string     Output format (text|json) (default "text")
      --passwd string     Pass word of sender (default "12345678")
      --trace             print out full stack trace on errors
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token burn 10000000bcoin-5b2 --fees=0.01tokt --from validator -b block
{
  "chain_id": "testing",
  "account_number": "1",
  "sequence": "6",
  "fee": {
    "amount": [
      {
        "denom": "tokt",
        "amount": "0.01000000"
      }
    ],
    "gas": "200000"
  },
  "msgs": [
    {
      "type": "okchain/token/MsgBurn",
      "value": {
        "amount": {
          "denom": "bcoin-5b2",
          "amount": "10000000.00000000"
        },
        "owner": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
      }
    }
  ],
  "memo": ""
}

confirm transaction before signing and broadcasting [y/N]: y
{
  "height": "6031107",
  "txhash": "47B08301B46DB848C411EA3CE668FB8EB5F0EB9779744F867B8A9068C26367D8",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":null}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": null
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "85074",
  "events": [
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "fee",
          "value": "10.01000000tokt"
        },
        {
          "key": "action",
          "value": "burn"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "okchain183rfa8tvtp6ax7jr7dfaf7ywv870sykx3uvcxn"
        },
        {
          "key": "amount",
          "value": "10000000.00000000bcoin-5b2"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "10.00000000tokt"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "0.01000000tokt"
        }
      ]
    }
  ]
}
```
## okchaincli tx token transfer-ownership
 - 转移代币的拥有权（需要多签）
- 帮助说明

```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token transfer-ownership -h
change the owner of the token

Usage:
  okchaincli tx token transfer-ownership [flags]

Flags:
  -a, --account-number uint     The account number of the signing account (offline mode only)
  -b, --broadcast-mode string   Transaction broadcasting mode (sync|async|block) (default "sync")
      --dry-run                 ignore the --gas flag and perform a simulation of a transaction, but don't broadcast it
      --fees string             Fees to pay along with transaction; eg: 1tokt
      --from string             Name or address of private key with which to sign
      --gas string              gas limit to set per-transaction; set to "auto" to calculate required gas automatically (default 200000) (default "200000")
      --gas-adjustment float    adjustment factor to be multiplied against the estimate returned by the tx simulation; if the gas limit is set manually this flag is ignored  (default 1)
      --gas-prices string       Gas prices to determine the transaction fee (e.g. 1tokt)
      --generate-only           Build an unsigned transaction and write it to STDOUT (when enabled, the local Keybase is not accessible and the node operates offline)
  -h, --help                    help for transfer-ownership
      --indent                  Add indent to JSON response
      --ledger                  Use a connected Ledger device
      --memo string             Memo to send along with transaction
      --node string             <host>:<port> to tendermint rpc interface for this chain (default "tcp://localhost:26657")
      --sequence uint           The sequence number of the signing account (offline mode only)
  -s, --symbol string           symbol of the token to be transferred
      --to string               the user to be transferred
      --trust-node              Trust connected full node (don't verify proofs for responses) (default true)
  -y, --yes                     Skip tx broadcasting prompt confirmation

Global Flags:
      --chain-id string   Chain ID of tendermint node
  -e, --encoding string   Binary encoding (hex|b64|btc) (default "hex")
      --home string       directory for config and data (default "/root/.okchaincli")
  -o, --output string     Output format (text|json) (default "text")
      --passwd string     Pass word of sender (default "12345678")
      --trace             print out full stack trace on errors
```
- 操作示例
```shell script
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token transfer-ownership --fees=0.01tokt --from okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm --to okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn --symbol bcoin-5b2 > unsignedTx.json
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx token multisigns unsignedTx.json --fees=0.01tokt --from lpl > signedTx1.json -y
root@iZwz9af3cg1abi4nmbogwxZ:~# cat signedTx1.json
{"type":"cosmos-sdk/StdTx","value":{"msg":[{"type":"okchain/token/MsgTransferOwnership","value":{"from_address":"okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm","to_address":"okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn","symbol":"bcoin-5b2","to_signature":{"pub_key":{"type":"tendermint/PubKeySecp256k1","value":"A6PAFVXrp3tJxf435Zfnvuw9yL9yO20oTs0Q6Mv0p2mQ"},"signature":"xxHYp7w5Z/cfjadGDhu070MVfQTx1BdMdNAZemOg2T9g9Yb3NY8QfTrKShVQFcH9nBxOlfzCaOXrDq8Gq4bZeA=="}}}],"fee":{"amount":[{"denom":"tokt","amount":"0.01000000"}],"gas":"200000"},"signatures":null,"memo":""}}
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx sign --fees=0.01tokt --from validator signedTx1.json > signedTx.json -y
root@iZwz9af3cg1abi4nmbogwxZ:~# cat signedTx.json
{
  "type": "cosmos-sdk/StdTx",
  "value": {
    "msg": [
      {
        "type": "okchain/token/MsgTransferOwnership",
        "value": {
          "from_address": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm",
          "to_address": "okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn",
          "symbol": "bcoin-5b2",
          "to_signature": {
            "pub_key": {
              "type": "tendermint/PubKeySecp256k1",
              "value": "A6PAFVXrp3tJxf435Zfnvuw9yL9yO20oTs0Q6Mv0p2mQ"
            },
            "signature": "xxHYp7w5Z/cfjadGDhu070MVfQTx1BdMdNAZemOg2T9g9Yb3NY8QfTrKShVQFcH9nBxOlfzCaOXrDq8Gq4bZeA=="
          }
        }
      }
    ],
    "fee": {
      "amount": [
        {
          "denom": "tokt",
          "amount": "0.01000000"
        }
      ],
      "gas": "200000"
    },
    "signatures": [
      {
        "pub_key": {
          "type": "tendermint/PubKeySecp256k1",
          "value": "Au3KpGHVDURp0V1fmU9rY2JIfr+ty35JgqYwWzC3MIHT"
        },
        "signature": "4X2JQSIFYS7oOTdkhmDeKtDgZZJ9AxPHNaNLhIXs72Aih42tQADtKCM7t+mcWDrnwy+6eKrVWf9YgKkcwgkLzg=="
      }
    ],
    "memo": ""
  }
}
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli tx broadcast signedTx.json -b block -y
{
  "height": "6038153",
  "txhash": "CAB3394945D74E3FCA09F4D4117D8D01458C5B5E04111E45F780270942783D43",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":null}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": null
    }
  ],
  "gas_wanted": "200000",
  "gas_used": "75080",
  "events": [
    {
      "type": "message",
      "attributes": [
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        },
        {
          "key": "module",
          "value": "token"
        },
        {
          "key": "fee",
          "value": "10.01000000tokt"
        },
        {
          "key": "action",
          "value": "transfer"
        },
        {
          "key": "sender",
          "value": "okchain1a8syfr3vdhrgu03nqzhj43u7v5ed8305m3g6sm"
        }
      ]
    },
    {
      "type": "transfer",
      "attributes": [
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "10.00000000tokt"
        },
        {
          "key": "recipient",
          "value": "okchain17xpfvakm2amg962yls6f84z3kell8c5ljresa7"
        },
        {
          "key": "amount",
          "value": "0.01000000tokt"
        }
      ]
    }
  ]
}
root@iZwz9af3cg1abi4nmbogwxZ:~# okchaincli query token info bcoin-5b2
{
  "description": "blockchain coin",
  "symbol": "bcoin-5b2",
  "original_symbol": "bcoin",
  "whole_name": "bcoin",
  "original_total_supply": "200000.00000000",
  "total_supply": "200000.00000000",
  "owner": "okchain179lql7dgsd5sf0h739sl0qlwncvpza3ge5audn",
  "mintable": true
}
```
## 复位
> 警告：不安全只有在开发中这样做，并且只有在您能够承受丢失所有区块链数据的代价时才这样做!

要重置区块链，请停止节点并运行：
```shell script
gaiad unsafe-reset-all
```
