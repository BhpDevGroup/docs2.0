## 安装docker-compose
docker-compose安装分为两种方式，官方的方式由于网络原因可能会导致下载失败，本文采用另一种方式，使用pip3安装。
- 官方方式
```shell script
https://docs.docker.com/compose/install/
```
- 使用pip3安装
```shell script
sudo apt-get install -y python3-pip
sudo pip3 install setuptools
sudo pip3 install --upgrade pip
sudo pip3 install docker-compose
```
查看docker-compose版本
```shell script
docker-compose version
```