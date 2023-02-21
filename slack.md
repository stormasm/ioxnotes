

### How does indexing work in Iox

Around 2/14/2023
pauldix

There's no real indexing in IOx. Data is organized into tables (measurement in InfluxDB), which are then split into partitions (by day by default, this will be user settable at some point in the future), and then individual Parquet files in those partitions. The parquet files are compacted in the background so that ideally they reach a certain size and are non-overlapping in the time ranges they contain.

Queries narrow down to the parquet files that need to be queried and execute in brute force across those.

So it would actually be <db>/<table>/<yyyy-mm-dd>/<chunk id>.parquet

btw, we already do some metadata indexing to avoid querying all parquet files. In the Catalog we keep the min and max times of the data in each parquet file. Time is the most effective discriminate for queries with our users so we keep that handy to rule out most parquet files without having to read them (or their metadata) to process a query

totally agree though, data skipping is the best way for these kinds of systems to optimize performance. Our users aren't asking for InfluxDB to churn through the entire data set. It's frequently a needle in a haystack kind of query

and I couldn't find anything about the metadata indexing you've mentioned, have you documented it somewhere?

We don't have it documented anywhere at this point. If you're trolling through the code it's the catalog that keeps that basic info (what tables exists, partitions, then parquet files beneath them. This is the relevant definition for what we keep on parquet files in there:

data_types/src/lib.rs

```rust
pub struct ParquetFile {
    /// the id of the file in the catalog
    pub id: ParquetFileId,
    /// the shard that sequenced writes that went into this file
    pub shard_id: ShardId,
```

### How Iox organizes parquet files in object store
Around 2/15/2023

How does iox organize the parquet files in object store? parquet file appears like this 1/1/2/10/b4d08c81-0397-46fa-a03c-87455661dd05.parquet
But what does the first numbers represent? I mean the numbers in the path. 1/1/2/10

Andrew Lamb
I believe the path is
namespace_id/table_id/shard_id/partition_id
see parquet_file/src/lib.rs for more details...

### Legacy Stuff

we have IOx deployed in one of our production environments in the new architecture configuration.

This breaks IOx up into 4 components (router, ingester, querier, compactor) that use Kafka, Postgres (for the catalog), and Object Store as shared infrastructure. This is all open source.

There's also an "all-in-one" mode that ties these together into a single process that can be run for easier integration testing (this is not used for benchmarking or production).

We intend to create a robust all-in-one mode later this year that eliminates the kafka and Postgres requirements and can operate using only a locally attached disk. This will be packaged up like InfluxDB as a single thing to run that gives the entire database in one package.

Our sequencing is something like:

Run in production in InfluxDB Cloud (using the 4 services mode)
Release alpha build of open source InfluxDB IOx Edge (the fast all in one mode) with documentation

Release on-premise commercial version of 4 services mode (for those that want to run our cloud database themselves)

<hr>

there are different operational modes for IOx.

There's the router, which sits in front of Kafka and receives writes and puts them in there.

The ingester which pulls data from Kafka, buffers it in memory, answers real-time queries for recent data and persists it to object store, the querier which handles queries and mixes data from object store and ingesters,

and the compactor which takes the parquet files in object store and compacts them together to clear out any deletes and optimize data organization for query performance.

IOx will also have a single server mode where everything is in a single process, uses a local WAL rather than Kafka, and can do all of those things.


I'm not familiar with Rockset's architecture, so I can't say. What we've landed on is a setup with 4 distinct components (these are IOx) and 3 shared services (these are other projects). Here's the list:

#### IOx Parts:

Router (has the public facing API, accepts writes, queries, etc.)

Ingester (pulls data from Kafka, which was put there by the Router, buffers in memory and persists Parquet files). Has a lower level query API for other components to query what is in the in memory buffer.

Querier (answers queries by combining data from Parquet files in object store with RPC queries to the Ingesters. Caches for performance)

Compactor (rewrites Parquet files from object store to optimize for query performance and to remove deletes)

#### Shared Services:

Kafka (this is the write buffer)

Object storage (S3, Azure, Google)
Postgres (this keeps the catalog of metadata about buckets, measurements, field types, parquet files, tombstones and other stuff)

### References
* [iox](./iox.md)
