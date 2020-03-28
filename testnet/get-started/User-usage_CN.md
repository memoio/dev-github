# MEFS 测试网使用

[TOC]

## 运行要求

docker 环境

## 获取账号

// contact xxx

## 获取 mefs 镜像

将 docker 镜像 pull 下来

```docker
// 获取
> sudo docker pull memoio/mefs-user:latest
```

## 启动 docker

- 启动 docker

```shell
// 启动docker; 4001用于网络连接，5080用于S3接口
> sudo docker run -itd -v <local datadir>:/root --name <container Name>  -p 4001:4001 -p 5080:5080 memoio/mefs-user:latest
```

- 进入终端

```shell
> sudo docker exec -it <container Name> bash
```

- 运行 check_user.sh，检查是否安装成功

```shell
/usr/local/bin/check_user.sh
```

## 启动 mefs

每个命令的参数解释见[使用文档](https://github.com/memoio/docs)

### user 初始化

```shell
> mefs-user init --netKey=testnet --sk=<your private key> --pwd=<your password>
```

参数解释：

- sk：私钥;
- pwd：密码，存储 keyfile;默认为"memoriae"

### 修改端口

修改默认的 4001 端口为<port num>，执行如下命令

````shell
// 运行daemon前执行
mefs-user config --json Addresses.Swarm "[\"/ip4/0.0.0.0/tcp/<port num>\"]"
```

### 启动 mefs 的实例，可后台运行

```shell
> mefs-user daemon --netKey=testnet --pwd=<your password> >> log 2>&1 &
```

### 用户空间 lfs 的使用

#### 启动用户

启动期间由于需要匹配合约，部署合约，耗时约 10~20 分钟

```shell
> mefs-user lfs start --pwd=<your password> --dur=<desired storage duration> --cap=<desired storage capacity> --price=<desired storage price> --ks=<desired keeper count> --ps=<desired provider count>
```

参数解释：

- dur: 提供的存储时间长度；按天计算， 默认是 100；
- cap：提供的存储大小；按 MB 计算，默认是 1000；
- price: 提供的存储价格，按 wei 计算，默认是 400000000000；即 3 美元/(TB\*月)
- ks: 需要的 keeper 的数目，默认是 2；
- ps：需要的 provider 的数目，默认是 6；

#### 创建桶

可以在该用户的加密存储空间 lfs 中新建 bucket。

```shell
> mefs-user lfs create_bucket bucket01 --pl=<redundance policy> --dc=<data count> --pc=<parity count>
```

参数解释：

- pl：数据冗余策略，1 为 RS 码， 2 为多副本，默认是 1；
- dc：数据块个数，默认为 3；dc+pc 不能大于 provider 的数目；
- pc：冗余块个数，默认为 2；

#### 上传测试文件

```shell
> mefs-user lfs put_object /localpath/test.dat bucket01
```

#### 下载文件，检验 md5 值是否正确

```shell
> mefs-user lfs get_object bucket01 test.dat
```

#### 查看 lfs 空间的信息

```shell
> mefs-user lfs show_storage
```

## S3 接口使用

### 启动 gateway

```shell
// 端口为5080
> mefs-user gateway start
```

s3 使用见文档
````
