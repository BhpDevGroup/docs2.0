## 部署私有的Gaia测试网
### 一、部署单节点
#### 要求
- [安装gaia](install-gaia.md )
#### 创建genesis.json文件并启动网络
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
### 二、部署多节点
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
> 您可能会看到一些连接错误，这没关系，P2P网络正在尝试查找可用的连接
> 可以添加几个社区公开节点到config.toml中的persistent_peers。

### 扩展
- [Gaia 部分命令使用指南](gaia.md )




