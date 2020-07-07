## Ethermint 部分命令使用指南
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
- 查看网络状态信息
```shell script
 netstat -nptl
```
- 导出私钥

```
emintcli keys unsafe-export-eth-key mykey
```