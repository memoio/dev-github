# MEFS Go Client SDK for Amazon S3 Compatible Cloud Storage

The MEFS Go Client SDK provides simple APIs to access any Amazon S3 compatible object storage.

This quickstart guide will show you how to install the MEFS client SDK, connect to MEFS, and provide a walkthrough for a simple file uploader. For a complete list of APIs and examples, please take a look at the [Go Client API Reference](https://docs.min.io/docs/golang-client-api-reference).

This document assumes that you have a working [Go development environment](https://golang.org/doc/install).

## Download from Github
```sh
GO111MODULE=on go get github.com/memoio/mefs-sdk-go
```

## Initialize MEFS Client
MEFS client requires the following four parameters specified to connect to an Amazon S3 compatible object storage.

| Parameter       | Description                                                      |
| :-------------- | :--------------------------------------------------------------- |
| endpoint        | URL to object storage service.                                   |
| accessKeyID     | Access key is the user ID that uniquely identifies your account. |
| secretAccessKey | Secret key is the password to your account.                      |
| secure          | Set this value to 'true' to enable secure (HTTPS) access.        |


```go
package main

import (
	"github.com/memoio/mefs-sdk-go"
	"log"
)

func main() {
	endpoint := "play.min.io"
	accessKeyID := "Q3AM3UQ867SPQQA43P2F"
	secretAccessKey := "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG"
	useSSL := true

	// Initialize mefs client object.
	mefsClient, err := mefs.New(endpoint, accessKeyID, secretAccessKey, useSSL)
	if err != nil {
		log.Fatalln(err)
	}

	log.Printf("%#v\n", mefsClient) // mefsClient is now setup
}
```

## Quick Start Example - File Uploader
This example program connects to an object storage server, creates a bucket and uploads a file to the bucket.

We will use the MEFS server running at [https://play.min.io](https://play.min.io) in this example. Feel free to use this service for testing and development. Access credentials shown in this example are open to the public.

### FileUploader.go
```go
package main

import (
	"github.com/memoio/mefs-sdk-go"
	"log"
)

func main() {
	endpoint := "play.min.io"
	accessKeyID := "Q3AM3UQ867SPQQA43P2F"
	secretAccessKey := "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG"
	useSSL := true

	// Initialize mefs client object.
	mefsClient, err := mefs.New(endpoint, accessKeyID, secretAccessKey, useSSL)
	if err != nil {
		log.Fatalln(err)
	}

	// Make a new bucket called mymusic.
	bucketName := "mymusic"
	location := "us-east-1"

	err = mefsClient.MakeBucket(bucketName, location)
	if err != nil {
		// Check to see if we already own this bucket (which happens if you run this twice)
		exists, errBucketExists := mefsClient.BucketExists(bucketName)
		if errBucketExists == nil && exists {
			log.Printf("We already own %s\n", bucketName)
		} else {
			log.Fatalln(err)
		}
	} else {
		log.Printf("Successfully created %s\n", bucketName)
	}

	// Upload the zip file
	objectName := "golden-oldies.zip"
	filePath := "/tmp/golden-oldies.zip"
	contentType := "application/zip"

	// Upload the zip file with FPutObject
	n, err := mefsClient.FPutObject(bucketName, objectName, filePath, mefs.PutObjectOptions{ContentType:contentType})
	if err != nil {
		log.Fatalln(err)
	}

	log.Printf("Successfully uploaded %s of size %d\n", objectName, n)
}
```

### Run FileUploader
```sh
go run file-uploader.go
2016/08/13 17:03:28 Successfully created mymusic
2016/08/13 17:03:40 Successfully uploaded golden-oldies.zip of size 16253413

mc ls play/mymusic/
[2016-05-27 16:02:16 PDT]  17MiB golden-oldies.zip
```

## API Reference
The full API Reference is available here.

* [Complete API Reference]()

### API Reference : Bucket Operations
* [`MakeBucket`](https://docs.min.io/docs/golang-client-api-reference#MakeBucket)
* [`ListBuckets`](https://docs.min.io/docs/golang-client-api-reference#ListBuckets)
* [`BucketExists`](https://docs.min.io/docs/golang-client-api-reference#BucketExists)
* [`RemoveBucket`](https://docs.min.io/docs/golang-client-api-reference#RemoveBucket)
* [`ListObjects`](https://docs.min.io/docs/golang-client-api-reference#ListObjects)

### API Reference : File Object Operations
* [`FPutObject`](https://docs.min.io/docs/golang-client-api-reference#FPutObject)
* [`FGetObject`](https://docs.min.io/docs/golang-client-api-reference#FGetObject)
* [`FPutObjectWithContext`](https://docs.min.io/docs/golang-client-api-reference#FPutObjectWithContext)
* [`FGetObjectWithContext`](https://docs.min.io/docs/golang-client-api-reference#FGetObjectWithContext)

### API Reference : Object Operations
* [`GetObject`](https://docs.min.io/docs/golang-client-api-reference#GetObject)
* [`PutObject`](https://docs.min.io/docs/golang-client-api-reference#PutObject)
* [`GetObjectWithContext`](https://docs.min.io/docs/golang-client-api-reference#GetObjectWithContext)
* [`PutObjectWithContext`](https://docs.min.io/docs/golang-client-api-reference#PutObjectWithContext)
* [`PutObjectStreaming`](https://docs.min.io/docs/golang-client-api-reference#PutObjectStreaming)
* [`StatObject`](https://docs.min.io/docs/golang-client-api-reference#StatObject)



### API Reference : Presigned Operations
* [`PresignedGetObject`](https://docs.min.io/docs/golang-client-api-reference#PresignedGetObject)
* [`PresignedPutObject`](https://docs.min.io/docs/golang-client-api-reference#PresignedPutObject)
* [`PresignedHeadObject`](https://docs.min.io/docs/golang-client-api-reference#PresignedHeadObject)
* [`PresignedPostPolicy`](https://docs.min.io/docs/golang-client-api-reference#PresignedPostPolicy)

### API Reference : Client custom settings
* [`SetAppInfo`](http://docs.min.io/docs/golang-client-api-reference#SetAppInfo)
* [`SetCustomTransport`](http://docs.min.io/docs/golang-client-api-reference#SetCustomTransport)
* [`TraceOn`](http://docs.min.io/docs/golang-client-api-reference#TraceOn)
* [`TraceOff`](http://docs.min.io/docs/golang-client-api-reference#TraceOff)

## Full Examples
