
This shutdown works prior to writing any data

```rust

2022-08-11T04:05:44.937474Z  INFO ioxd_common: Received SIGINT

2022-08-11T04:05:44.937547Z  INFO ioxd_common: Received SIGINT

2022-08-11T04:05:44.937570Z  INFO ioxd_common: Shutdown requested server_type=Querier

2022-08-11T04:05:44.937588Z  INFO ioxd_common: Shutdown requested server_type=Compactor

2022-08-11T04:05:44.937474Z  INFO ioxd_common: Received SIGINT

2022-08-11T04:05:44.937657Z  INFO ioxd_common: Shutdown requested server_type=Ingester

2022-08-11T04:05:44.937728Z  INFO ioxd_common: Shutdown requested server_type=Router

2022-08-11T04:05:44.938857Z  INFO ioxd_common: HTTP server shutdown server_type=Ingester

2022-08-11T04:05:44.938856Z  INFO ioxd_common: HTTP server shutdown server_type=Querier

2022-08-11T04:05:44.938924Z  INFO ioxd_common: frontend shutdown completed server_type=Ingester

2022-08-11T04:05:44.938970Z DEBUG hyper::server::shutdown: signal received, starting graceful shutdown

2022-08-11T04:05:44.939153Z  INFO ingester::lifecycle: Lifecycle manager shutdown

2022-08-11T04:05:44.938926Z  INFO ioxd_common: frontend shutdown completed server_type=Querier

2022-08-11T04:05:44.939029Z DEBUG hyper::server::shutdown: signal received, starting graceful shutdown

2022-08-11T04:05:44.940135Z  INFO ioxd_common: HTTP server shutdown server_type=Router

2022-08-11T04:05:44.940213Z  INFO ioxd_common: frontend shutdown completed server_type=Router

2022-08-11T04:05:44.940396Z  INFO ingester::stream_handler::handler: stream handler shutdown kafka_topic=iox-shared kafka_partition=0

2022-08-11T04:05:44.940728Z  INFO ioxd_common: backend shutdown completed server_type=Router

2022-08-11T04:05:44.941005Z  INFO ioxd_common: backend shutdown completed server_type=Querier

2022-08-11T04:05:44.941318Z  INFO ioxd_common: backend shutdown completed server_type=Ingester

2022-08-11T04:05:44.944622Z  INFO ioxd_common: gRPC server shutdown server_type=Compactor

2022-08-11T04:05:44.944678Z  INFO ioxd_common: frontend shutdown completed server_type=Compactor

2022-08-11T04:05:44.945291Z  INFO influxdb_iox::commands::run::main: done serving, draining futures grpc_bind_address=SocketAddr(127.0.0.1:8081) http_bind_address=Some(SocketAddr(127.0.0.1:8080)) server_type=Router

2022-08-11T04:05:44.945737Z  INFO influxdb_iox::commands::run::main: done serving, draining futures grpc_bind_address=SocketAddr(127.0.0.1:8082) http_bind_address=None server_type=Querier

2022-08-11T04:05:44.946213Z DEBUG influxdb_iox::commands::run::main: handle returned server_type=Router

2022-08-11T04:05:44.946280Z DEBUG influxdb_iox::commands::run::main: wait for handle server_type=Ingester

2022-08-11T04:05:44.946423Z  INFO influxdb_iox::commands::run::main: done serving, draining futures grpc_bind_address=SocketAddr(127.0.0.1:8083) http_bind_address=None server_type=Ingester

2022-08-11T04:05:44.948555Z DEBUG influxdb_iox::commands::run::main: handle returned server_type=Ingester

2022-08-11T04:05:44.948734Z DEBUG influxdb_iox::commands::run::main: wait for handle server_type=Compactor

2022-08-11T04:05:45.506206Z  INFO ioxd_common: backend shutdown completed server_type=Compactor

2022-08-11T04:05:45.506327Z  INFO influxdb_iox::commands::run::main: done serving, draining futures grpc_bind_address=SocketAddr(127.0.0.1:8084) http_bind_address=None server_type=Compactor

2022-08-11T04:05:45.512271Z DEBUG influxdb_iox::commands::run::main: handle returned server_type=Compactor

2022-08-11T04:05:45.512349Z DEBUG influxdb_iox::commands::run::main: wait for handle server_type=Querier

2022-08-11T04:05:45.512389Z DEBUG influxdb_iox::commands::run::main: handle returned server_type=Querier
```




### Legacy Notes

So it breaks on the commit:

* [run many compact partitions in parallel](https://github.com/influxdata/influxdb_iox/pull/5230)

Now I can figure out what to do...



### Works here

* [July 12](https://github.com/influxdata/influxdb_iox/pull/5110) :
 [pr5110](https://github.com/influxdata/influxdb_iox/commit/ad4ea13e9d3c51cab1febc770c51d22a05855ac6)
 
 * [July 19](https://github.com/influxdata/influxdb_iox/pull/5134) :
 [pr5134](https://github.com/influxdata/influxdb_iox/commit/b8d9799a268634b4a8226554c8950a480b9760b0)
 
 * [July 25](https://github.com/influxdata/influxdb_iox/pull/5206) :
 [pr5206](https://github.com/influxdata/influxdb_iox/commit/d05f383a986414c6576a2abcb2a34fe5bb1a8b25)
 
 * [July 26](https://github.com/influxdata/influxdb_iox/pull/5210) :
 [pr5210](https://github.com/influxdata/influxdb_iox/commit/de22b6b080b66d89f0c44cccdf1b9dd7ed124b9f)
 
 * [July 27](https://github.com/influxdata/influxdb_iox/pull/5225) :
 [pr5225](https://github.com/influxdata/influxdb_iox/commit/7eebe061a69b20430d911383a199b381c4e4fbfa) **fix: reduce log verbosity for found compaction candidates message**
 
 ### This is where it breaks pr5230
 
 * [July 27](https://github.com/influxdata/influxdb_iox/pull/5230) :
 [pr5230](https://github.com/influxdata/influxdb_iox/commit/fcce00bf0921622eb1d1334865b2b8fc5fa6f7ed) **feat: run many compact partitions in parallel**
 
 * [July 28](https://github.com/influxdata/influxdb_iox/pull/5229) :
 [pr5229](https://github.com/influxdata/influxdb_iox/commit/9215a534d0fd241442c40db995884b6d899800eb)
 
 * [July 28](https://github.com/influxdata/influxdb_iox/pull/5235) :
 [pr5235](https://github.com/influxdata/influxdb_iox/commit/0e9695f20273818502e6c8b8f49c02f6449aca17)