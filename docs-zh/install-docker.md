## 在Ubuntu系统上安装Docker
- 卸载旧版本
```shell script
sudo apt-get remove docker docker-engine docker.io containerd runc
```
- 更新索引库
```shell script
sudo apt-get update
```
- 安装一些必要的系统工具（安装以下包以使apt可以通过HTTPS使用存储库）
```shell script
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```
- 添加Docker官方的GPG密钥或者阿里云的
```shell script
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```
- 写入软件源信息
```shell script
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```
- 更新并安装 Docker-CE
```shell script
sudo apt-get -y update
sudo apt-get -y install docker-ce
```
- 验证docker运行状态
```shell script
systemctl status docker
```
- 使用阿里云加速器
>使用加速器（采用阿里云加速器）
>https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

###参考文档
```shell script
https://docs.docker.com/engine/install/ubuntu/
```
