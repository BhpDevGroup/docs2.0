## 部署自己的Gaia测试网
### 部署单节点
```shell script
# You can run all of these commands from your home directory
cd $HOME

# Initialize the genesis.json file that will help you to bootstrap the network
gaiad init --chain-id=testing testing

# Create a key to hold your validator account
gaiacli keys add validator

# Add that key into the genesis.app_state.accounts array in the genesis file
# NOTE: this command lets you set the number of coins. Make sure this account has some coins
# with the genesis.app_state.staking.params.bond_denom denom, the default is staking
gaiad add-genesis-account $(gaiacli keys show validator -a) 1000000000stake,1000000000validatortoken

# Generate the transaction that creates your validator
gaiad gentx --name validator

# Add the generated bonding transaction to the genesis file
gaiad collect-gentxs

# Now its safe to start `gaiad`
gaiad start
```
### 部署多节点
#### 要求
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
启动4个网络节点
```shell script
make localnet-start
```
此命令使用gaiadnode映像创建4个节点网络。下表中是每个节点对应的端口：

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
#### 结构
该`make localnet-start`部署四个节点的测试网络会在`./build`中调用`gaiad testnet`命令，然后在 `./build` 目录下输入文件:
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
#### 密钥 & 账户
返回此密钥管理器存储的所有密钥的名称、类型、地址和公钥列表。
```shell script
gaiacli keys list --home ./build/node0/gaiacli
```


### 复位
> 警告：不安全只有在开发中这样做，并且只有在您能够承受丢失所有区块链数据的代价时才这样做!

要重置区块链，请停止节点并运行：
```shell script
gaiad unsafe-reset-all
```

### 普通节点加入自己部署的测试网络
```shell script
gaiad init <your_custom_moniker> --chain-id=chain-G7L8cJ
gaiad start
```




