# MEFS TestNet

This is the repository of the MEFS testnet. The following will explain how to join the MEFS testnet.  
The update of the MEFS testnet will also be explained here.

## Notice

The MEFS testnet is divided into three roles, user `User`, maintainer `Keeper`, and storer `Provider`. Please see [document](/testnet/get-started/) for specific usage.

## Testnet News

### Version

v0.3.2

### user

#### completed

- Storage order: query-contract deployment, you can set the required storage duration, size and price；
- Store order matching；
- Sign storage order and deploy upkeeping contract;
- User space LFS;
- Upload and download functions;

#### added

- Download payment；
- Upkeeping contract renewal to improve security;
- S3 interface;
- Data format optimization;
- The hash value of the user space is stored on the chain;
- Users share files;

### keeper

#### completed

- Setting of role contract;
- Public verification and challenge of user data;
- Repair of user data；
- Time and space payment for storage order;
- Delay in issuing user time-space payment;
- Save the metadata of user;

#### added

- Store orders are matched in the dimensions of price, space and time;

#### in progress

- The addition of multi-raft;
- Optimize the repair in the mining pool mode;

### provider

#### completed

- Role-contract settings;
- Storage market: the deployment of offer contracts, setting the space, duration, and price you can provide;
- User's data storage;
- Response to challenges to user data;
- Repair of user data;
- Pos function of data cold start;

#### added

- Pledge space;
- Verification of pos data volume;
- Data migration before exit;
- Provider's revenue display;

## Resources

- Code：https://github.com/memoio/testnet （not open yet）
- Document：https://github.com/memoio/docs
