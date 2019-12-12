# MEFS Go Client API Reference

## Initialize MEFS Client object

##  MEFS

```go
package main

import (
    "fmt"

    "github.com/memoio/mefs-sdk-go"
)

func main() {
        // Use a secure connection.
        ssl := true

        // Initialize mefs client object.
        mefsClient, err := mefs.New("play.min.io", "Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG", ssl)
        if err != nil {
                fmt.Println(err)
                return
        }
}
```

## AWS S3

```go
package main

import (
    "fmt"

    "github.com/memoio/mefs-sdk-go"
)

func main() {
        // Use a secure connection.
        ssl := true

        // Initialize mefs client object.
        s3Client, err := mefs.New("s3.amazonaws.com", "YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY", ssl)
        if err != nil {
                fmt.Println(err)
                return
        }
}
```

| Bucket operations                          | Object operations                                   |
| :---                                       | :---                                                |
| [`MakeBucket`](#MakeBucket)                | [`GetObject`](#GetObject)                           |
| [`ListBuckets`](#ListBuckets)              | [`PutObject`](#PutObject)                           |
| [`BucketExists`](#BucketExists)            | [`StatObject`](#StatObject)                         |
| [`RemoveBucket`](#RemoveBucket)            | [`RemoveObject`](#RemoveObject)                     |
| [`ListObjects`](#ListObjects)              | |
## 1. Constructor
<a name="MEFS"></a>

### New(endpoint, accessKeyID, secretAccessKey string, ssl bool) (*Client, error)
Initializes a new client object.

__Parameters__

|Param   |Type   |Description   |
|:---|:---| :---|
|`endpoint`   | _string_  |S3 compatible object storage endpoint   |
|`accessKeyID`  |_string_   |Access key for the object storage |
|`secretAccessKey`  | _string_  |Secret key for the object storage |
|`ssl`   | _bool_  | If 'true' API requests will be secure (HTTPS), and insecure (HTTP) otherwise  |

### NewWithRegion(endpoint, accessKeyID, secretAccessKey string, ssl bool, region string) (*Client, error)
Initializes mefs client, with region configured. Unlike New(), NewWithRegion avoids bucket-location lookup operations and it is slightly faster. Use this function when your application deals with a single region.

### NewWithOptions(endpoint string, options *Options) (*Client, error)
Initializes mefs client with options configured.

__Parameters__

|Param   |Type   |Description   |
|:---|:---| :---|
|`endpoint`   | _string_  |S3 compatible object storage endpoint |
|`opts`  |_mefs.Options_   | Options for constructing a new client|

__mefs.Options__

|Field | Type | Description |
|:--- |:--- | :--- |
| `opts.Creds` | _*credentials.Credentials_ | Access Credentials|
| `opts.Secure` | _bool_ | If 'true' API requests will be secure (HTTPS), and insecure (HTTP) otherwise |
| `opts.Region` | _string_ | region |
| `opts.BucketLookup` | _BucketLookupType_ | Bucket lookup type can be one of the following values |
| |  | _mefs.BucketLookupDNS_ |
| |  | _mefs.BucketLookupPath_ |
| |  | _mefs.BucketLookupAuto_ |
## 2. Bucket operations

<a name="MakeBucket"></a>
### MakeBucket(bucketName, location string) error
Creates a new bucket.

__Parameters__

| Param  | Type  | Description  |
|---|---|---|
|`bucketName`  | _string_  | Name of the bucket |
| `location`  |  _string_ | |


__Example__


```go
err = mefsClient.MakeBucket("mybucket", "us-east-1")
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println("Successfully created mybucket.")
```

<a name="ListBuckets"></a>
### ListBuckets() ([]BucketInfo, error)
Lists all buckets.

| Param  | Type  | Description  |
|---|---|---|
|`bucketList`  | _[]mefs.BucketInfo_  | Lists of all buckets |


__mefs.BucketInfo__

| Field  | Type  | Description  |
|---|---|---|
|`bucket.Name`  | _string_  | Name of the bucket |
|`bucket.CreationDate`  | _time.Time_  | Date of bucket creation |


__Example__


```go
buckets, err := mefsClient.ListBuckets()
if err != nil {
    fmt.Println(err)
    return
}
for _, bucket := range buckets {
    fmt.Println(bucket)
}
```

<a name="BucketExists"></a>
### BucketExists(bucketName string) (found bool, err error)
Checks if a bucket exists.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName`  | _string_  |Name of the bucket |


__Return Values__

|Param   |Type   |Description   |
|:---|:---| :---|
|`found`  | _bool_ | Indicates whether bucket exists or not  |
|`err` | _error_  | Standard Error  |


__Example__


```go
found, err := mefsClient.BucketExists("mybucket")
if err != nil {
    fmt.Println(err)
    return
}
if found {
    fmt.Println("Bucket found")
}
```

<a name="RemoveBucket"></a>
### RemoveBucket(bucketName string) error
Removes a bucket, bucket should be empty to be successfully removed.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName`  | _string_  |Name of the bucket   |

__Example__


```go
err = mefsClient.RemoveBucket("mybucket")
if err != nil {
    fmt.Println(err)
    return
}
```

<a name="ListObjects"></a>
### ListObjects(bucketName, prefix string, recursive bool, doneCh chan struct{}) <-chan ObjectInfo
Lists objects in a bucket.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName` | _string_  |Name of the bucket   |
|`objectPrefix` |_string_   | Prefix of objects to be listed |
|`recursive`  | _bool_  |`true` indicates recursive style listing and `false` indicates directory style listing delimited by '/'.  |
|`doneCh`  | _chan struct{}_ | A message on this channel ends the ListObjects iterator.  |


__Return Value__

|Param   |Type   |Description   |
|:---|:---| :---|
|`objectInfo`  | _chan mefs.ObjectInfo_ |Read channel for all objects in the bucket, the object is of the format listed below: |

__mefs.ObjectInfo__

|Field   |Type   |Description   |
|:---|:---| :---|
|`objectInfo.Key`  | _string_ |Name of the object |
|`objectInfo.Size`  | _int64_ |Size of the object |
|`objectInfo.ETag`  | _string_ |MD5 checksum of the object |
|`objectInfo.LastModified`  | _time.Time_ |Time when object was last modified |

```go
// Create a done channel to control 'ListObjects' go routine.
doneCh := make(chan struct{})

// Indicate to our routine to exit cleanly upon return.
defer close(doneCh)

isRecursive := true
objectCh := mefsClient.ListObjects("mybucket", "myprefix", isRecursive, doneCh)
for object := range objectCh {
    if object.Err != nil {
        fmt.Println(object.Err)
        return
    }
    fmt.Println(object)
}
```

## 3. Object operations

<a name="GetObject"></a>
### GetObject(bucketName, objectName string, opts GetObjectOptions) (*Object, error)
Returns a stream of the object data. Most of the common errors occur when reading the stream.


__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName`  | _string_  |Name of the bucket  |
|`objectName` | _string_  |Name of the object  |
|`opts` | _mefs.GetObjectOptions_ | Options for GET requests specifying additional options like encryption, If-Match |


__mefs.GetObjectOptions__

|Field | Type | Description |
|:---|:---|:---|
| `opts.ServerSideEncryption` | _encrypt.ServerSide_ | Interface provided by `encrypt` package to specify server-side-encryption. (For more information see https://godoc.org/github.com/mefs/mefs-go/v6) |

__Return Value__


|Param   |Type   |Description   |
|:---|:---| :---|
|`object`  | _*mefs.Object_ |_mefs.Object_ represents object reader. It implements io.Reader, io.Seeker, io.ReaderAt and io.Closer interfaces. |


__Example__


```go
object, err := mefsClient.GetObject("mybucket", "myobject", mefs.GetObjectOptions{})
if err != nil {
    fmt.Println(err)
    return
}
localFile, err := os.Create("/tmp/local-file.jpg")
if err != nil {
    fmt.Println(err)
    return
}
if _, err = io.Copy(localFile, object); err != nil {
    fmt.Println(err)
    return
}
```

<a name="PutObject"></a>
### PutObject(bucketName, objectName string, reader io.Reader, objectSize int64,opts PutObjectOptions) (n int, err error)
Uploads objects that are less than 128MiB in a single PUT operation. For objects that are greater than 128MiB in size, PutObject seamlessly uploads the object as parts of 128MiB or more depending on the actual file size. The max upload size for an object is 5TB.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName`  | _string_  |Name of the bucket  |
|`objectName` | _string_  |Name of the object   |
|`reader` | _io.Reader_  |Any Go type that implements io.Reader |
|`objectSize`| _int64_ |Size of the object being uploaded. Pass -1 if stream size is unknown |
|`opts` | _mefs.PutObjectOptions_  | Allows user to set optional custom metadata, content headers, encryption keys and number of threads for multipart upload operation. |

__mefs.PutObjectOptions__

|Field | Type | Description |
|:--- |:--- | :--- |
| `opts.UserMetadata` | _map[string]string_ | Map of user metadata|
| `opts.Progress` | _io.Reader_ | Reader to fetch progress of an upload |
| `opts.ContentType` | _string_ | Content type of object, e.g "application/text" |
| `opts.ContentEncoding` | _string_ | Content encoding of object, e.g "gzip" |
| `opts.ContentDisposition` | _string_ | Content disposition of object, "inline" |
| `opts.ContentLanguage` | _string_ | Content language of object, e.g "French" |
| `opts.CacheControl` | _string_ | Used to specify directives for caching mechanisms in both requests and responses e.g "max-age=600"|
| `opts.ServerSideEncryption` | _encrypt.ServerSide_ | Interface provided by `encrypt` package to specify server-side-encryption. (For more information see https://godoc.org/github.com/mefs/mefs-go/v6) |
| `opts.StorageClass` | _string_ | Specify storage class for the object. Supported values for MEFS server are `REDUCED_REDUNDANCY` and `STANDARD` |
| `opts.WebsiteRedirectLocation` | _string_ | Specify a redirect for the object, to another object in the same bucket or to a external URL. |

__Example__


```go
file, err := os.Open("my-testfile")
if err != nil {
    fmt.Println(err)
    return
}
defer file.Close()

fileStat, err := file.Stat()
if err != nil {
    fmt.Println(err)
    return
}

n, err := mefsClient.PutObject("mybucket", "myobject", file, fileStat.Size(), mefs.PutObjectOptions{ContentType:"application/octet-stream"})
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println("Successfully uploaded bytes: ", n)
```

API methods PutObjectWithSize, PutObjectWithMetadata, PutObjectStreaming, and PutObjectWithProgress available in mefs-go SDK release v3.0.3 are replaced by the new PutObject call variant that accepts a pointer to PutObjectOptions struct.

<a name="PutObjectWithContext"></a>
### PutObjectWithContext(ctx context.Context, bucketName, objectName string, reader io.Reader, objectSize int64, opts PutObjectOptions) (n int, err error)
Identical to PutObject operation, but allows request cancellation.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`ctx`  | _context.Context_  |Request context |
|`bucketName`  | _string_  |Name of the bucket  |
|`objectName` | _string_  |Name of the object   |
|`reader` | _io.Reader_  |Any Go type that implements io.Reader |
|`objectSize`| _int64_ | size of the object being uploaded. Pass -1 if stream size is unknown |
|`opts` | _mefs.PutObjectOptions_  |Pointer to struct that allows user to set optional custom metadata, content-type, content-encoding, content-disposition, content-language and cache-control headers, pass encryption module for encrypting objects, and optionally configure number of threads for multipart put operation. |


__Example__


```go
ctx, cancel := context.WithTimeout(context.Background(), 10 * time.Second)
defer cancel()

file, err := os.Open("my-testfile")
if err != nil {
    fmt.Println(err)
    return
}
defer file.Close()

fileStat, err := file.Stat()
if err != nil {
    fmt.Println(err)
    return
}

n, err := mefsClient.PutObjectWithContext(ctx, "my-bucketname", "my-objectname", file, fileStat.Size(), mefs.PutObjectOptions{
	ContentType: "application/octet-stream",
})
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println("Successfully uploaded bytes: ", n)
```

<a name="StatObject"></a>
### StatObject(bucketName, objectName string, opts StatObjectOptions) (ObjectInfo, error)
Fetch metadata of an object.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName`  | _string_  |Name of the bucket  |
|`objectName` | _string_  |Name of the object   |
|`opts` | _mefs.StatObjectOptions_ | Options for GET info/stat requests specifying additional options like encryption, If-Match |


__Return Value__

|Param   |Type   |Description   |
|:---|:---| :---|
|`objInfo`  | _mefs.ObjectInfo_  |Object stat information |


__mefs.ObjectInfo__

|Field   |Type   |Description   |
|:---|:---| :---|
|`objInfo.LastModified`  | _time.Time_  |Time when object was last modified |
|`objInfo.ETag` | _string_ |MD5 checksum of the object|
|`objInfo.ContentType` | _string_ |Content type of the object|
|`objInfo.Size` | _int64_ |Size of the object|


__Example__


```go
objInfo, err := mefsClient.StatObject("mybucket", "myobject", mefs.StatObjectOptions{})
if err != nil {
    fmt.Println(err)
    return
}
fmt.Println(objInfo)
```

<a name="RemoveObject"></a>
### RemoveObject(bucketName, objectName string) error
Removes an object.

__Parameters__


|Param   |Type   |Description   |
|:---|:---| :---|
|`bucketName`  | _string_  |Name of the bucket  |
|`objectName` | _string_  |Name of the object |


```go
err = mefsClient.RemoveObject("mybucket", "myobject")
if err != nil {
    fmt.Println(err)
    return
}
```

