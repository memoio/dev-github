# MEFS测试网

这是MEFS测试网的仓库，以下将说明如何加入MEFS测试网。  
MEFS测试网的更新动态也会在这里说明

## 入门指南

MEFS测试网分为三个角色，用户`User`、维护者`Keeper`、存储者`Provider`，具体使用请看[文档](/testnet/get-started/How-to-install.md)

## 测试网动态

### user

#### 完成

- 存储订单：query 合约部署，可以设置需要的存储时长，大小和价格；
- 订单匹配；
- 存储订单签订，upkeeping 合约部署；
- 用户空间 LFS；
- 上传和下载功能；

#### 进展中

- 下载支付；
- upkeeping 合约的续约

### keeper

#### 完成

- 角色合约的设置；
- 矿池模式，即 provider 可以设置自己的主 keeper；
- user 的元数据保存；
- user 数据的 PDP 公开验证；
- user 数据的修复；
- 存储订单的时空支付；

#### 进展中

- 矿池模式下的修复优化；
- 存储订单匹配，当前只匹配价格；后续考虑加入空间和时间维度；
- keeper 退出机制

### provider

#### 完成

- 角色合约的设置
- 存储市场：offer 合约的部署，设置自己可以提供的空间，时长，以及价格；
- user 的数据保存；
- user 数据的挑战相应；
- user 数据的修复；
- 数据冷启动的 pos 功能；

#### 进展中

- 退出前的数据迁移

## 现有资源

* 源码：https://github.com/memoio/testnet 
* 文档：https://github.com/memoio/docs
