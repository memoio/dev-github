# MEFS 命令行操作文档

[TOC]

## 命令详情

### 启动

#### 初始化

mefs 初始化，默认初始化的目录为\$HOME/.mefs，可以通过 export MEFS_PATH=<local dir>的方式设置初始化的目录，然后再运行 init。

```shell
> mefs-keeper init --netKey=<net key> --sk=<your private key> --pwd=<your password> --keyfile=<absolute path of your keyfile>
```

参数解释：

- sk：私钥地址，不指定时默认新建一个账户；
- pwd：密码，不指定时为默认密码；
- keyfile: keyfile 文件的完整路径，不指定时默认为新建一个keyfile；
- netKey: 私有网络标志，现在有 dev 和 testnet；同一个标志的网络可以互联，默认为dev。

#### 修改网络传输端口

默认为 4001 端口，若需要改为<port num>，执行如下命令；
keeper 要求此 <port num> 可以被直接访问，或者在外网机器上有端口映射。

```shell
// 运行daemon前执行
mefs-keeper config --json Addresses.Swarm "[\"/ip4/0.0.0.0/tcp/<port num>\"]"
```

例如改为 4090 端口：

```shell
// 运行daemon前执行
mefs-keeper config --json Addresses.Swarm "[\"/ip4/0.0.0.0/tcp/4090\"]"
```

#### 启动实例

```shell
> mefs-keeper daemon --netKey= <net key> --pwd=<your password> --sk=<your secretKey>
```

参数解释：

- netKey: 私有网络标志，现在有 dev 和 testnet；同一个标志的网络可以互联，默认为dev；
- pwd：密码，不指定时为默认密码；
- sk：私钥地址，不指定时默认从本地私钥文件导出；