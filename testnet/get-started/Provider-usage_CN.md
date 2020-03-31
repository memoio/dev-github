# MEFS 测试网使用

[TOC]

## 运行要求

docker 环境，外网 ip，4001 端口开放

## 获取账号

// contact xxx

## 注册 provider

// visit xxx

## 获取 mefs 镜像

将 docker 镜像 pull 下来

```shell
// 获取
> sudo docker pull memoio/mefs-provider:latest
```

## 启动 docker

- 启动 docker

要求：机器的 4001 端口可以被公网访问，或者在出口机器上有端口映射

```docker
// 启动docker; 4001用于网络连接
> sudo docker run -itd -v <local datadir>:/root --name <container Name> -p 4001:4001 memoio/mefs-provider:latest
```

- 进入终端

```shell
// 进入docker
> sudo docker exec -it <container Name> bash
```

- 运行 check_provider_mcl.sh，检查是否安装成功；由于指令集的差异，这里可能会重新编译一些动态库。

```shell
/usr/local/bin/check_provider_mcl.sh
```

## 启动 mefs

每个命令的参数解释见[使用文档](https://github.com/memoio/docs)

### mefs 初始化

```shell
> mefs-provider init --netKey=testnet --sk=<your private key> --pwd=<your password>
```

参数解释：

- sk：私钥;
- pwd：密码，存储 keyfile;默认为"memoriae"

### 修改端口

修改默认的 4001 端口为<port num>，执行如下命令；要求此 port num 可以被直接访问，或者在外网机器上有端口映射。

````shell
// 运行daemon前执行
mefs-provider config --json Addresses.Swarm "[\"/ip4/0.0.0.0/tcp/<port num>\"]"
```

### 启动 mefs 的实例，可后台运行

```shell
> mefs-provider daemon --netKey=testnet --pwd=<your password> --dur=<your storage duration> --cap=<your storage capacity> --price=<your storage price> --deCap=<your deposit capacity> --rdo=<bool> --pos=true
````

参数解释：

- dur: 提供的存储时间长度；按天计算， 默认是 365 天；
- cap：提供的存储空间大小；按 MB 计算，默认是 100000MB；
- price: 提供的存储价格，按 wei 计算，默认是 400000000000 wei；即 3 美元/(TB\*月)
- rdo：是否重新部署 offer 合约，默认是 false。
- deCap：质押的空间；按 MB 计算，默认 10000；
- pos：是否使用冷启动功能，默认是 false，不开启；若使用，需要保证可用空间大于质押的空间
