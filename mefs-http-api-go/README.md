# MEFS HTTP API

A client library for the MEFS HTTP API, implemented in JavaScript. This client library implements the interface-mefs-core. In addition, this client library implements a set of utility functions.

## Table of Contents

- [Install](#install)
  - [Running the daemon with the right port](#running-the-daemon-with-the-right-port)
  - [Importing the module and usage](#importing-the-module-and-usage)
  - [In a web browser](#in-a-web-browser)
  - [CORS](#cors)
- [Usage](#usage)
  - [LFS](#lfs)
- [License](#license)

## Install

This module uses node.js, and can be installed through npm:

```shell
npm install mefs-http-api-js
```

We support both the Current and Active LTS versions of Node.js. Please see [nodejs.org](https://nodejs.org/) for what these currently are.

### Running the daemon with the right port

To interact with the API, you need to have a local daemon running. It needs to be open on the right port. `5001` is the default, and is used in the examples below, but it can be set to whatever you need.

```shell
# Show the mefs config API port
> mefs config Addresses.API
/ip4/127.0.0.1/tcp/5001
# set api port and binding to all ip 
> mefs config Addresses.API /ip4/0.0.0.0/tcp/5001
# Restart the daemon after changing the config
> mefs shutdown
# Run the daemon
> mefs daemon
```

### Importing the module and usage

```javascript
var mefsClient = require('mefs-http-api-js')

// connect to mefs daemon API server
var mefs = mefsClient('localhost', '5001', { protocol: 'http' }) // leaving out the arguments will default to these values

// or connect with multiaddr
var mefs = mefsClient('/ip4/127.0.0.1/tcp/5001')

// or using options
var mefs = mefsClient({ host: 'localhost', port: '5001', protocol: 'http' })

// or specifying a specific API path
var mefs = mefsClient({ host: '1.1.1.1', port: '80', 'api-path': '/mefs/api/v0' })
```

### In a web browser

Instead of a local installation (and browserification) you may request a remote copy of MEFS API from [unpkg CDN](https://unpkg.com/).

To always request the latest version, use the following:

```html
<script src="https://unpkg.com/mefs-http-api-js/dist/bundle.js"></script>
```

### CORS

In a web browser mefs HTTP client (either browserified or CDN-based) might encounter an error saying that the origin is not allowed. This would be a CORS ("Cross Origin Resource Sharing") failure: mefs servers are designed to reject requests from unknown domains by default. You can whitelist the domain that you are calling from by changing your mefs config like this:

```shell
> mefs config --json API.HTTPHeaders.Access-Control-Allow-Origin  '["http://example.com"]'
> mefs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "POST", "GET"]'
# Restart the daemon after changing the config
> mefs shutdown
# Run the daemon
> mefs daemon
```

## Usage

### LFS

- The  API enables users to use the LFS abstraction of MEFS.
  - [create_bucket](#create_bucket)
  - [delete_bucket](#delete_bucket)
  - [delete_object](#delete_object)
  - [fsync](#fsync)
  - [get_object](#get_object)
  - [head_bucket](#head_bucket)
  - [head_object](#head_object)
  - [kill](#kill)
  - [list_buckets](#list_buckets)
  - [list_keepers](#list_keepers)
  - [list_objects](#list_objects)
  - [list_providers](#list_providers)
  - [put_object_from_file_system](#put_object_from_file_system)
  - [show_storage](#show_storage)
  - [start](#start)

#### `create_bucket`

> create a bucket in lfs.

##### `mefs.lfs.create_bucket([bucketname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name we want.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.
- `pl` Policy. The Storage policy you want to use. Type is `bool`. `true`for erasure coding and `false` for copyset. `default` is `true`.
- `dc` DataCount. `default` is 3.
- `pc` ParityCount. `default` is 2.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'Create Bucket',
  Buckets: [
    {
      BucketName: 'bucket03',
      BucketID: 4,
      Ctime: '2019-06-04 Tue 14:54:11 CST',
      Policy: true,
      DataCount: 3,
      ParityCount: 2
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.create_bucket("bucket03",function (err, createBucketInfo) {
    if (err) {
        throw err
    }
    console.log(createBucketInfo)
})
```

#### `delete_bucket`

> delete a bucket in lfs.

##### `mefs.lfs.delete_bucket([bucketname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name .

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'Delete Bucket',
  Buckets: [
    {
      BucketName: 'bucket03',
      BucketID: 4,
      Ctime: '2019-06-04 Tue 14:54:11 CST',
      Policy: true,
      DataCount: 3,
      ParityCount: 2
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.delete_bucket("bucket03",function (err, deleteBucketInfo) {
    if (err) {
        throw err
    }
    console.log(deleteBucketInfo)
})
```

#### `delete_object`

> delete an object in a bucket.

##### `mefs.lfs.delete_object([bucketname],[objectname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name.
`objectname` is a string of the object name.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID.The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'Delete Object',
  Objects: [
    {
      ObjectName: '2.txt',
      ObjectSize: 401,
      MD5: '800b746fdc4d80469afc7e5e9b510c9c',
      Ctime: '2019-06-03 Mon 20:52:50 CST',
      Dir: false,
      LatestChalTime: ''
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.delete_object("bucket01","2.txt",function (err, deleteObjectInfo) {
    if (err) {
        throw err
    }
    console.log(deleteObjectInfo)
})
```

#### `fsync`

> flush lfs's metadata.

##### `mefs.lfs.fsync([option],function (err, createBucketInfo)`

`options` is an optional object argument that might include the following keys:

- `addr` AddressID.The actual user's addressid that you want to execute. Type is `string`.
- `f` ForceFlush. Type is `bool`. `default` is `false`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be a string of:

```JavaScript
'Flush Success'
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.fsync(function (err, fsyncInfo) {
    if (err) {
        throw err
    }
    console.log(fsyncInfo)
})
```

#### `get_object`

> get an object in a bucket.

##### `mefs.lfs.get_object([bucketname],[objectname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name.
`objectname` is a string of the object name.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of the object in the `byte` form:

```JavaScript
< 68 65 6c 6c 6f 0a>
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.get_object("bucket01","aa",function (err, file) {
    if (err) {
        throw err
    }
    console.log(file)
})
```

#### `head_bucket`

> print the information of a bucket.

##### `mefs.lfs.head_bucket([bucketname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'Head Bucket',
  Buckets: [
    {
      BucketName: 'bucket03',
      BucketID: 4,
      Ctime: '2019-06-04 Tue 14:54:11 CST',
      Policy: true,
      DataCount: 3,
      ParityCount: 2
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.head_bucket("bucket01",function (err, headBucketInfo) {
    if (err) {
        throw err
    }
    console.log(headBucketInfo)
})
```

#### `head_object`

> print the information of an object in a bucket.

##### `mefs.lfs.head_object([bucketname],[objectname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name.
`objectname` is a string of the object name.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID.The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'Head Object',
  Objects: [
    {
      ObjectName: 'aa',
      ObjectSize: 6,
      MD5: '754bb514d8fc70da46afc9ce9ce50469',
      Ctime: '2019-06-03 Mon 15:09:38 CST',
      Dir: false,
      LatestChalTime: '2019-06-04 Tue 15:45:47 CST'
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.head_object("bucket01","aa",function (err, headObjectInfo) {
    if (err) {
        throw err
    }
    console.log(headObjectInfo)
})
```

#### `kill`

> stop user's service

##### `mefs.lfs.kill([addr],[option],function (err, createBucketInfo)`

`addr` AddressID. Stop user service with the given address. Type is `string`.

`options` is an optional object argument that might include the following keys:

- `pwd` PassWord. Password of the actual user that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{ ChildLists: [ 'Kill User: [user id]' ] }
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.kill("0x739AF155Eb2e46F7592ed737fC8eE5D09D41b4dE",function (err, killInfo) {
    if (err) {
        throw err
    }
    console.log(killInfo)
})
```

#### `list_buckets`

> print informations of all buckets in lfs.

##### `mefs.lfs.list_buckets([option],function (err, createBucketInfo)`

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'List Buckets',
  Buckets: [
    {
      BucketName: 'bucket02',
      BucketID: 3,
      Ctime: '2019-06-04 Tue 09:53:00 CST',
      Policy: true,
      DataCount: 3,
      ParityCount: 2
    },
    {
      BucketName: 'bucket01',
      BucketID: 1,
      Ctime: '2019-06-03 Mon 13:49:19 CST',
      Policy: true,
      DataCount: 3,
      ParityCount: 2
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.list_buckets(function (err, listBucketsInfo) {
    if (err) {
        throw err
    }
    console.log(listBucketsInfo)
})
```

#### `list_keepers`

> print the information of keepers that the user uses

##### `mefs.lfs.list_keepers([option],function (err, createBucketInfo)`

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  ChildLists: [
    '[keeper id]  connected',
    '[keeper id]  connected',
    '[keeper id]  connected',
    '[keeper id]  connected'
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.list_keepers(function (err, listKeepersInfo) {
    if (err) {
        throw err
    }
    console.log(listKeepersInfo)
})
```

#### `list_objects`

> print informations of all objects of a bucket in lfs

##### `mefs.lfs.list_objects([bucketname],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  Method: 'List Objects',
  Objects: [
    {
      ObjectName: 'aa',
      ObjectSize: 6,
      MD5: '754bb514d8fc70da46afc9ce9ce50469',
      Ctime: '2019-06-03 Mon 15:09:38 CST',
      Dir: false,
      LatestChalTime: '2019-06-04 Tue 15:54:37 CST'
    },
    {
      ObjectName: '1.txt',
      ObjectSize: 2978,
      MD5: '4a469c0b56baf19e57fdc54c7d04c8e9',
      Ctime: '2019-06-04 Tue 10:00:50 CST',
      Dir: false,
      LatestChalTime: '2019-06-04 Tue 15:54:37 CST'
    }
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.list_objects("bucket01",function (err, listObjectsInfo) {
    if (err) {
        throw err
    }
    console.log(listObjectsInfo)
})
```

#### `list_providers`

> print the information of providers that the user uses

##### `mefs.lfs.list_providers([option],function (err, createBucketInfo)`

`options` is an optional object argument that might include the following keys:

- `addr` AddressID.The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  ChildLists: [
    '[provider id]  connected',
    '[provider id]  connected',
    '[provider id]  connected',
    '[provider id]  connected',
    '[provider id]  connected',
    '[provider id]  connected'
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.list_providers(function (err, listProvidersInfo) {
    if (err) {
        throw err
    }
    console.log(listProvidersInfo)
})
```

#### `put_object_from_file_system`

> put an object to a bucket

##### `mefs.lfs.put_object_from_file_system([bucketname],[path],[option],function (err, createBucketInfo)`

`bucketname` is a string of the bucket name.
`path` is a string of the object name.

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.
- `args` The bucket we want to put. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
[ { Method: 'Put Object', Objects: [ [Object] ] } ]
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.put_object_from_file_system("bucket01","/mnt/d/4.txt",function (err, file) {
    if (err) {
        throw err
    }
    console.log(file)
})
```

#### `show_storage`

> show the used storage space

##### `mefs.lfs.show_storage([option],function (err, createBucketInfo)`

`options` is an optional object argument that might include the following keys:

- `addr` AddressID. The actual user's addressid that you want to execute. Type is `string`.

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be a number of the space used.

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.show_storage(function (err, showStorageInfo) {
    if (err) {
        throw err
    }
    console.log(showStorageInfo)
})
```

#### `start`

> start user's service 

##### `mefs.lfs.start([addr],[option],function (err, createBucketInfo)`

`addr` AddressID. Initialize user service with the given address. Type is `string`.

`options` is an optional object argument that might include the following keys:

- `pwd` PassWord. Password of the actual user that you want to execute. Type is `string`.
- `sk` SecreteKey. Private key of the actual user that you want to execute. Type is `string`. If `sk` is not `nil`, mefs will store `sk` in the keystore with `pwd`; otherwise, mefs tries to load the private key from the keystore use the `addr` and `pwd`.  

`callback` must follow `function (err, res) {}` signature, where `err` is an error if the operation was not successful. `res` will be an array of:

```JavaScript
{
  ChildLists: [
    'Begin to start User, the address is : ' +
      '0xF14D13cabE8167c95Bc1672166adc6cDD80E61a0'
  ]
}
```

If no `callback` is passed, a promise is returned.

**Example:**

```JavaScript
mefs.lfs.start("0xF14D13cabE8167c95Bc1672166adc6cDD80E61a0",function (err, startInfo) {
    if (err) {
        throw err
    }
    console.log(startInfo)
})
```

## License

[MIT](LICENSE)