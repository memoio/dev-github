# MEFS 命令行操作文档

[TOC]

## 命令详情

### 启动

#### 初始化

mefs 初始化，默认初始化的目录为\$HOME/.mefs，可以通过 export MEFS_PATH=<local dir>的方式设置初始化的目录，然后再运行 init。

```shell
> mefs-provider init --netKey=<net key> --sk=<your private key> --pwd=<your password> --keyfile=<absolute path of your keyfile>
```

参数解释：

- sk：私钥地址；
- pwd：密码；
- keyfile: keyfile 文件的完整路径；
- netKey: 私有网络标志，现在有 dev 和 testnet；同一个标志的网络可以互联。

#### 修改网络传输端口

默认为 4001 端口，若需要改为<port num>，执行如下命令；
provider 要求此 <port num> 可以被直接访问，或者在外网机器上有端口映射。

```shell
// 运行daemon前执行
mefs-provider config --json Addresses.Swarm "[\"/ip4/0.0.0.0/tcp/<port num>\"]"
```

例如改为 4090 端口：

```shell
// 运行daemon前执行
mefs-provider config --json Addresses.Swarm "[\"/ip4/0.0.0.0/tcp/4090\"]"
```

#### 启动实例

```shell
> mefs-provider daemon --netKey= <net key> --pwd=<your password> --dur=<your storage duration> --cap=<your storage capacity> --price=<your storage price> --deCap=<your deposit capacity> --rdo=<bool> --pos=true
```

参数解释：

- dur: 提供的存储时间长度；按天计算， 默认是 365 天；
- cap：提供的存储空间大小；按 MB 计算，默认是 100000MB；
- price: 提供的存储价格，按 wei 计算，默认是 400000000000 wei；即 3 美元/(TB\*月)；
- rdo：是否重新部署 offer 合约，默认是 false；
- deCap：质押的空间；按 MB 计算，默认 10000；
- pos：是否使用冷启动功能，默认是 false，不开启；若使用，需要保证可用空间大于质押的空间；
- netKey: 私有网络标志，现在有 dev 和 testnet；同一个标志的网络可以互联；
