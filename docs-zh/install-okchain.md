## 安装Go

按照[官方文档](https://golang.org/doc/install )安装go。
> Cosmos SDK需要Go 1.14+。

记得设置您的$GOPATH，$GOBIN和$PATH环境变量，例如：
```shell script
mkdir -p $HOME/go/bin
echo "export GOPATH=$HOME/go" >> ~/.bashrc
echo "export GOBIN=$GOPATH/bin" >> ~/.bashrc
echo "export PATH=$PATH:$GOBIN" >> ~/.bashrc
source ~/.bashrc
```
查看go是否安装成功
```shell script
go version
```
## 安装okchain
安装最新版的 okchain. 确保已经已经切换到正确的[发行版本](https://github.com/okex/okchain/releases )。
>请确保您的服务器可以访问 google.com，因为我们的项目依赖于google提供的某些库（如果您无法访问google.com，也可以尝试添加代理：export GOPROXY=https://goproxy.io）
```shell script
git clone -b <latest-release-tag> https://github.com/okex/okchain
cd okchain && make install
```
如果环境变量配置无误，则通过运行以上命令即可完成gaia的安装。现在检查您的gaia版本是否正确：
```shell script
okchaind version --long
okchaincli version --long
```
会输出以下类似内容
```shell script
{
	"name": "okchain",
	"server_name": "okchaind",
	"client_name": "okchaincli",
	"version": "v0.10.9",
	"commit": "3262dfd664f70a33c951aed3ca9d0224a6bae65d",
	"build_tags": "netgo",
	"go": "go version go1.13.3 linux/amd64",
	"cosmos_sdk": "v0.37.9",
	"tendermint": "v0.32.10"
}
```