
[chore: Allow running all-in-one with external object store #5600](https://github.com/influxdata/influxdb_iox/pull/5600)

```rust
ioxd
minio server ./data
```

##### mc - MinIO Client for object storage and filesystems.

```rust
ioxd
mc ls
```

go to [http://localhost:9000](http://localhost:9000)      
login with minioadmin / minioadmin    
and create a bucket called **lowcard**

```rust
export AWS_ACCESS_KEY_ID="minioadmin"
export AWS_SECRET_ACCESS_KEY="minioadmin"
export AWS_ENDPOINT="http://localhost:9000"
```

```rust
alias ioxaws='cargo run --features aws -- run all-in-one --object-store s3 --aws-endpoint http://localhost:9000 --bucket lowcard --aws-allow-http --log-filter debug,compactor=error'
```

### References

[Rust Client for Minio](https://github.com/minio/minio-rs)

### From homebrew installation

Command-line Access: https://docs.min.io/docs/minio-client-quickstart-guide

Object API (Amazon S3 compatible):  
Go:         https://docs.min.io/docs/golang-client-quickstart-guide  
Java:       https://docs.min.io/docs/java-client-quickstart-guide  
Python:     https://docs.min.io/docs/python-client-quickstart-guide  
JavaScript: https://docs.min.io/docs/javascript-client-quickstart-guide  
.NET:       https://docs.min.io/docs/dotnet-client-quickstart-guide

Talk to the community: https://slack.min.io  
==> Get started:  
NAME:  
minio server - start object storage server  

USAGE:  
minio server [FLAGS] DIR1 [DIR2..]  
minio server [FLAGS] DIR{1...64}  
minio server [FLAGS] DIR{1...64} DIR{65...128}  

DIR:
DIR points to a directory on a filesystem. When you want to combine
multiple drives into a single large system, pass one directory per
filesystem separated by space. You may also use a '...' convention
to abbreviate the directory arguments. Remote directories in a
distributed setup are encoded as HTTP(s) URIs.

```rust
FLAGS:
--address value              bind to a specific ADDRESS:PORT, ADDRESS can be an IP or hostname (default: ":9000") [$MINIO_ADDRESS]
--listeners value            bind N number of listeners per ADDRESS:PORT (default: 1) [$MINIO_LISTENERS]
--console-address value      bind to a specific ADDRESS:PORT for embedded Console UI, ADDRESS can be an IP or hostname [$MINIO_CONSOLE_ADDRESS]
--certs-dir value, -S value  path to certs directory (default: "/Users/ma/.minio/certs")
--quiet                      disable startup and info messages
--anonymous                  hide sensitive information from logging
--json                       output logs in JSON format
--help, -h                   show help
```

EXAMPLES:  
1. Start minio server on "/home/shared" directory.  
$ minio server /home/shared

2. Start single node server with 64 local drives "/mnt/data1" to "/mnt/data64".    
$ minio server /mnt/data{1...64}

3. Start distributed minio server on an 32 node setup with 32 drives each, run following command on all the nodes 
```rust
$ export MINIO_ROOT_USER=minio  
$ export MINIO_ROOT_PASSWORD=miniostorage  
$ minio server http://node{1...32}.example.com/mnt/export{1...32}  
```

4. Start distributed minio server in an expanded setup, run the following command on all the nodes
```rust
$ export MINIO_ROOT_USER=minio
$ export MINIO_ROOT_PASSWORD=miniostorage
$ minio server http://node{1...16}.example.com/mnt/export{1...32} \
http://node{17...64}.example.com/mnt/export{1...64}
```

🍺  /usr/local/Cellar/minio/RELEASE.2022-09-07T22-25-02Z_1: 3 files, 100MB, built in 3 seconds

