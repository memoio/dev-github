# 基于MinIO的MEFS Go SDK

MinIO Go Client SDK提供了简单的API来访问任何与Amazon S3兼容的对象存储服务。MEFS提供了基于MinIO的SDK。

**支持的云存储:** 

- AWS Signature Version 4
   - Amazon S3
   - MinIO

- AWS Signature Version 2
   - Google Cloud Storage (兼容模式)
   - Openstack Swift + Swift3 middleware
   - Ceph Object Gateway
   - Riak CS

本文我们将学习如何安装MinIO client SDK，连接到MinIO，并提供一下文件上传的示例。对于完整的API以及示例，请参考[s3-sdk-go](/docs/api/s3-sdk-go.md)。

本文假设你已经有 [Go开发环境](https://golang.org/doc/install) 以及 [MEFS](***代码位置或者二进制文件下载位置)、[MEFS账号](***加入MEFS网络教程)

## 从Github下载
```sh
go get -u github.com/minio/minio-go
```

## 运行MEFS
开启一个终端，运行MEFS daemon
```sh
mefs daemon
```
再开启一个终端，启动lfs
```sh
mefs lfs start
mefs gateway start
```

## 初始化MinIO Client
MinIO client需要以下4个参数来连接与Amazon S3兼容的对象存储。

| 参数  | 描述| 
| :---         |     :---     |
| endpoint   | MEFS服务的endpoint   |
| accessKeyID | MEFS账户的地址 |
| secretAccessKey | MEFS账户密码 |
| secure | true代表使用HTTPS |


```go
package main

import (
    "fmt"

    "github.com/minio/minio-go"
)

func main() {
    endpoint := "127.0.0.1:5080"
    accessKeyID := "0x6A6AEB9840C8a42A9d8Ff55cf2c213F9b812ED0A"
    secretAccessKey := "123456789"
    useSSL := false

    _, err := minio.New(endpoint, accessKeyID, secretAccessKey, useSSL)
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println("OK")
}
```

## API文档
完整的API文档在这里。
* [完整API文档](/docs/api/s3-sdk-go.md)

## 了解更多
* [完整文档](/docs/README.md)
