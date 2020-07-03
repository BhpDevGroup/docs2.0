## 安装Ethermint
### 准备环境
首先，通过运行以下命令确保您的系统和apt包列表完全更新：

```
apt-get update -y
apt-get upgrade -y
```
### 安装golang
```
curl -O https://dl.google.com/go/go1.14.3.linux-amd64.tar.gz
tar -xvf go1.14.3.linux-amd64.tar.gz
mv go /usr/local
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile

mkdir goApps
echo "export GOPATH=/root/goApps" >> ~/.profile
echo "export PATH=\$PATH:\$GOPATH/bin" >> ~/.profile
source ~/.profile
```
设置go代理
```
export GOPROXY=https://goproxy.io
```
### 安装make
```
apt install make
```
### 安装git
```
apt install git
```
检验git是否安装成功
```
git version
```
### 下载源码
下载源码，可选择github或者码云gitee下载。
github：
```
https://github.com/chainsafe/ethermint
git clone https://github.com/chainsafe/ethermint.git
```
gitee：
```
https://gitee.com/holechain/ethermint/
git clone https://gitee.com/holechain/ethermint.git
```
查看项目代码是否是在`development`分支。
```
git branch
```
如果不是切换到`development`分支。
```
git checkout development
```
### 编译
在 `ethermint`目录下安装
>请确保您的服务器可以访问 google.com，因为我们的项目依赖于google提供的某些库（如果您无法访问google.com，也可以尝试添加代理：export GOPROXY=https://goproxy.io）
```
make install
```
构建二进制文件并将其放到 ./build 目录下
```
make build
```