Everytime you update
[stormasm/influxdb_iox](https://github.com/stormasm/influxdb_iox)

with the command

```rust
alias gpiox='git pull https://github.com/influxdata/influxdb_iox main'
```

if you get an error like this one

```rust
Server command failed: Catalog error: database setup error: while executing migrations: error returned from database: column "column_set" of relation "parquet_file" contains null values
```

If you have any issues and the db was already installed and you have to drop the database run this command

```rust
psql postgres
drop database iox_shared
```

Run the following two commands at the top level influxdb_iox

* ioxdbcreate
* ioxdbstart

For more details go
[here](https://github.com/stormasm/ioxnotes/blob/main/startup.md) or
[here](https://github.com/influxdata/influxdb_iox/tree/main/iox_catalog)

So there are two different repos working with each other...

1) There is the main repo

2) There is my repo with the iox sql client

Step 1 is to turn off the logging in the main repo so I can see what is going on without having a bunch of log messages rolling along

To bring up the main repo or server

```rust
alias iox='ioxg; ./target/debug/influxdb_iox'
alias ioxinfo='iox run all-in-one --log-filter info'
alias ioxdebug='iox run all-in-one --log-filter debug'
alias ioxdebugnoh2='iox run all-in-one --log-filter debug,hyper::proto::h1=info,h2=info'
```

Step 2 is to know how to drive the iox sql client...

It can be driven in two places

* in the main repo
* in alternate repos as well as eventually the nushell command

In the main repo bring up the iox sql client like this

```rust
alias ioxsql='iox sql'
```

Once inside the iox sql client type these commands, note eventually I will build my own client possibly based on nushell...

```rust
use postgresql:///iox_shared;
select * from h2o_temperature;
set format {json, csv, pretty};
```

For more details go
[here](./query.md)

For an alternative iox sql client go
[here](https://github.com/stormasm/iox_sql_v00)

### How to suppress logging in the main repo

##### compactor/src/compact.rs

turn off debug! for
* Number of candidate partitions to be considered to compact

##### compactor/src/handler.rs

turn off
* debug!(n_candidates, "found compaction candidates");

##### ingester/src/stream_handler/periodic_watermark_fetcher.rs

turn off
* debug!("fetching write buffer watermark");

### Details on how to load data into the database

* [query.md](https://github.com/stormasm/ioxnotes/blob/main/query.md)
