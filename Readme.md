
[Iox Commands](https://github.com/stormasm/ioxnotes/blob/main/commands.md)

Everytime you update
[stormasm/influxdb_iox](https://github.com/stormasm/influxdb_iox)

with the command

```rust
alias gpiox='git pull https://github.com/influxdata/influxdb_iox main'
```

### Make the following possible code changes

* [code changes](./codechanges.md)

### INFLUXDB_IOX_DB_DIR

To see where this is referenced check this out

```rust
ioxg
rg INFLUXDB_IOX_DB_DIR
```

The current value of this environment variable can be gotten this way

```rust
env | grep INFLUXDB_IOX
```

To clean up this directory simply go to this directory

```rust
ioxd
INFLUXDB_IOX_DB_DIR=/Users/ma/j/tmp22/iox_datadir
```

And everything below this directory gets blown away

```rust
rm -fr *
```

if you get an error like this one

```rust
Server command failed: Catalog error: database setup error: while executing migrations: error returned from database: column "column_set" of relation "parquet_file" contains null values
```

If you have any issues and the db was already installed and you have to drop the database run this command

```rust
psql postgres
drop database iox_shared
\list
\q
```

Run the following two commands at the top level influxdb_iox

* ioxdbcreate
* ioxdbstart

For more details go
[here](https://github.com/stormasm/ioxnotes/blob/main/startup.md) or
[here](https://github.com/influxdata/influxdb_iox/tree/main/iox_catalog)

### After you have iox up and running

Bring up iox by simply typing **iox**

```rust
alias ioxg='cd ~/j/tmp06/influxdb_iox'
alias iox='ioxg; ./target/debug/influxdb_iox'
```

Bring up the Iox client by simply typing **ioxsql**

```rust
alias ioxsql='iox sql'
```
* show namespaces;

There should be no namespaces at this moment in time, proving to you that you are starting with a clean slate :smile:

Write the data out to Iox

```rust
alias ioxwtemp='ioxg; iox write postgresql:///iox_shared ./test_fixtures/lineproto/temperature.lp --host http://localhost:8081'
```

Now review the data you just wrote out to Iox

* show namespaces;
* use postgresql:///iox_shared;
* show tables;
* select * from h2o_temperature;

### More details

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

### And finally modify this code...

to reduce 5 minutes to 30 seconds

clap_blocks/src/ingester.rs

```rust
/// If a partition has had data buffered and hasn't received a write for this
/// period of time, it will be persisted. The default value is 300 seconds (5 minutes).
#[clap(
    long = "--persist-partition-cold-threshold-seconds",
    env = "INFLUXDB_IOX_PERSIST_PARTITION_COLD_THRESHOLD_SECONDS",
    default_value = "30",
    action
)]
pub persist_partition_cold_threshold_seconds: u64,
```

influxdb_iox/src/commands/run/all_in_one.rs

```rust
/// If a partition has had data buffered and hasn't received a write for this
/// period of time, it will be persisted. The default value is 300 seconds (5 minutes).
#[clap(
    long = "--persist-partition-cold-threshold-seconds",
    env = "INFLUXDB_IOX_PERSIST_PARTITION_COLD_THRESHOLD_SECONDS",
    default_value = "30",
    action
)]
pub persist_partition_cold_threshold_seconds: u64,
```

or more simply add this line to your .iox file

```rust
export INFLUXDB_IOX_PERSIST_PARTITION_COLD_THRESHOLD_SECONDS='30'
```

### How is data organized

* database (bucket)
* tables
* partitions
* parquet files

Database is the top level entity in Iox 

Databases contain one or more tables representing measurement.  All data within a table has the same schema

Tables contain collections of partitions.  partitions are indexed by time.  Used for data lifecycle management.  You can drop whole partitions to evict
aged data.

The parquet files contains the compressed data in columnar format.  The Compactor manages the size and count of parquet files by not allowing them
to be too big or too many.

Data is both HOT and COLD.  The hot data is in memory and represents 
seconds, minutes, hours...  The cold data is in object store and represents weeks, months and years.  Data in memory is optimized for low latency while
the data in object storage is optimized for lowest cost long term storage.

the at-rest data format is Parquet (stored in S3)   
the in-memory data structures are Arrow   
the query protocol is FlightSQL   

The big difference between TSM and IOX, in terms of "data layout", is what a physical column represents. TSM stores and handles series as columns (1000 series => 1000 columns on disk and 1000 columns in memory), where IOx stores and handles timestamps, fields and tags as columns (1 timestamp, 10 tags, 20 columns => 31 columns on disk and 31 columns in memory).

IOx does have a write-ahead log, and the data in the WAL is queryable as soon it lands in the WAL.
