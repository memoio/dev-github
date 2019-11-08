# 如何安装

## 安装与启动

### 安装

- 使用 docker

```docker
> docker pull memoio/mefs
> docker run -itd -v <datadir>:/root/.mefs <image>
```

### 初始化

mefs 初始化，默认初始化的目录为\$HOME/.mefs，可以通过 export MEFS_PATH=<localpath>的方式设置初始化的目录，然后再运行 init。

```shell
> mefs init --sk=<your private key> --pwd=<your password>
```

参数解释：

- sk：私钥地址；
- pwd：密码；

## mefs 使用说明

- mefs 有三类角色: user, keeper, provider

user 使用存储空间，需要为 provider 的存储、keeper 的管理进行支付
provider 负责存储数据
keeper 负责管理：运行挑战，触发支付

## 账户申请与注册

1. 每个角色通过私钥（private key）唯一标识，对外用公钥（public key）作为名字，私钥用密码（password）加密后存放于 keystore 目录下；
2. 角色需要在链上注册，mefs 启动的时候会根据所属角色启动不同的服务，一个地址只运行为一个角色。