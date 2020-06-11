## Ethermint节点集群
Ethermint可选用手动初始化搭建多节点

删除现有的守护程序`.emintd`和客户端`.emintcli`
```
rm -rf ~/.emint*
```
设置Ethermint的网络别名和chain-id(生成`.emintd`目录，初始化配置文件`config`和数据`data`)
```
emintd init node0 --chain-id 8
```
参数设置
```
emintcli config chain-id 8
emintcli config output json
emintcli config indent true
emintcli config trust-node true
emintcli config keyring-backend test
```
签署创始交易的密钥：(代码中的mykey替换成自己定义的字段,比如换成：bhptest2)
```
emintcli keys add bhptest2
```
分配创始帐户额度
```
emintd add-genesis-account $(emintcli keys show bhptest2 -a) 1000000000000000000photon,1000000000000000000stake
emintd gentx --name bhptest2 --keyring-backend test
```
查看创始交易集合
```
emintd collect-gentxs
```
验证有效性
```
emintd validate-genesis
```
获取本节点node-id
```
emintd tendermint show-node-id
```
![获取本节点node-id](./images/ncoe-1.png) 

多节点启动
```
emintd start --p2p.persistent_peers="f5088aa26cf82be7657977603d5a8f9f9cfae267@127.0.0.1:26606,ddb4df78fe5bdf120e8ba9468857765756948e18@127.0.0.1:26616,6bb7f6e1ef06ab114708e421ec84352f5f203e21@127.0.0.1:26626,13db9b7388f2192bd65ded3cdf1216ad7e17e6db@127.0.0.1:26636"
```

单节点启动（如果不需要历史查询可移除`--pruning=nothing`）
```
emintd start --pruning=nothing --rpc.unsafe --log_level "main:info,state:info,mempool:info" --trace
```
复制
```shell script
cp -r /root/.emintd /home/testnode/000/
cp -r /root/.emintcli /home/testnode/000/
```

第一个节点配置文件生成
```shell script
emintd init node0 --chain-id 8 --home /home/ethermint/testnode/genesis_data/node0
emintcli config chain-id 8 --home  /home/ethermint/testnode/genesis_data/emintcli
emintcli config output json --home  /home/ethermint/testnode/genesis_data/emintcli
emintcli config indent true --home /home/ethermint/testnode/genesis_data/emintcli
emintcli config trust-node true --home /home/ethermint/testnode/genesis_data/emintcli
emintcli config keyring-backend test --home  /home/ethermint/testnode/genesis_data/emintcli
                                                                                 
emintcli keys add bhptest0 --home  /home/ethermint/testnode/genesis_data/emintcli

emintd add-genesis-account $(emintcli keys show bhptest0 -a --home /home/ethermint/testnode/genesis_data/emintcli) 1000000000000000000photon,1000000000000000000stake --home /home/ethermint/testnode/genesis_data/node0

emintd gentx --name bhptest0 --keyring-backend test --home /home/ethermint/testnode/genesis_data/node0
```
如果出现这个错误
```
ERROR: failed to read from keybase: The specified item could not be found in the keyring
```
把/home/ethermint/testnode/genesis_data/emintcli目录复制到/root/.emintcli
```
cp -r /home/ethermint/testnode/genesis_data/emintcli/* /root/.emintcli/
```
```
emintd collect-gentxs --home /home/ethermint/testnode/genesis_data/node0
emintd validate-genesis --home /home/ethermint/testnode/genesis_data/node0
emintd tendermint show-node-id --home /home/ethermint/testnode/genesis_data/node0
```

第二个节点
```shell script
emintcli config keyring-backend test --home  /home/ethermint/testnode/genesis_data/emintcli
emintcli config chain-id 8 --home  /home/ethermint/testnode/genesis_data/emintcli
emintcli config output json --home  /home/ethermint/testnode/genesis_data/emintcli
emintcli config indent true --home /home/ethermint/testnode/genesis_data/emintcli
emintcli config trust-node true --home /home/ethermint/testnode/genesis_data/emintcli

emintd init node1 --chain-id 8 --home /home/ethermint/testnode/genesis_data/node1
emintcli keys add bhptest1 --home  /home/ethermint/testnode/genesis_data/emintcli
emintd add-genesis-account $(emintcli keys show bhptest1 -a --home  /home/ethermint/testnode/genesis_data/emintcli) 1000000000000000000photon,1000000000000000000stake --home /home/ethermint/testnode/genesis_data/node1
emintd gentx --name bhptest1 --keyring-backend test --home /home/ethermint/testnode/genesis_data/node1
emintd collect-gentxs --home /home/ethermint/testnode/genesis_data/node1
emintd validate-genesis --home /home/ethermint/testnode/genesis_data/node1
emintd tendermint show-node-id --home /home/ethermint/testnode/genesis_data/node1
```

第一个节点
```
rm -rf ~/.emint*
./emintd init node0 --chain-id 8
./emintcli config chain-id 8
./emintcli config output json
./emintcli config indent true
./emintcli config trust-node true
./emintcli config keyring-backend test
./emintcli keys add bhptest0

./emintd add-genesis-account $(emintcli keys show bhptest0 -a) 1000000000000000000photon,1000000000000000000stake
./emintd gentx --name bhptest0 --keyring-backend test
./emintd collect-gentxs
./emintd validate-genesis
./emintd tendermint show-node-id

```
第二个节点
```
rm -rf ~/.emint*
./emintd init node1 --chain-id 8
./emintcli config chain-id 8
./emintcli config output json
./emintcli config indent true
./emintcli config trust-node true
./emintcli config keyring-backend test
./emintcli keys add bhptest1

./emintd add-genesis-account $(emintcli keys show bhptest1 -a) 1000000000000000000photon,1000000000000000000stake
./emintd gentx --name bhptest1 --keyring-backend test
./emintd collect-gentxs
./emintd validate-genesis
./emintd tendermint show-node-id
```
第三个节点
```
rm -rf ~/.emint*
./emintd init node2 --chain-id 8
./emintcli config chain-id 8
./emintcli config output json
./emintcli config indent true
./emintcli config trust-node true
./emintcli config keyring-backend test
./emintcli keys add bhptest2

./emintd add-genesis-account $(emintcli keys show bhptest2 -a) 1000000000000000000photon,1000000000000000000stake
./emintd gentx --name bhptest2 --keyring-backend test
./emintd collect-gentxs
./emintd validate-genesis
./emintd tendermint show-node-id
```
第四个节点
```shell script
rm -rf ~/.emint*
./emintd init node3 --chain-id 8
./emintcli config chain-id 8
./emintcli config output json
./emintcli config indent true
./emintcli config trust-node true
./emintcli config keyring-backend test
./emintcli keys add bhptest3

./emintd add-genesis-account $(emintcli keys show bhptest3 -a) 1000000000000000000photon,1000000000000000000stake
./emintd gentx --name bhptest3 --keyring-backend test
./emintd collect-gentxs
./emintd validate-genesis
./emintd tendermint show-node-id

nohup ./emintd start --home genesis_data/node1 --rpc.laddr tcp://0.0.0.0:26657 > eth001.log &
nohup ./emintd start --home genesis_data/node0 --rpc.laddr tcp://0.0.0.0:26657 > eth000.log &
tail -f eth000.log

./emintd start --p2p.persistent_peers="a683a213e0b9ad3e6a6ae0c2e82822bc2a850cff@101.133.151.154:26556,af8d443e2b35f0cb0b9201c10d2ce66fec8caf4f@101.133.151.154:26666" --rpc.laddr tcp://0.0.0.0:26657 --home genesis_data/node0

./emintd start --p2p.persistent_peers="a683a213e0b9ad3e6a6ae0c2e82822bc2a850cff@101.133.151.154:26556,af8d443e2b35f0cb0b9201c10d2ce66fec8caf4f@101.133.151.154:26666" --rpc.laddr tcp://0.0.0.0:26667 --home genesis_data/node1
```