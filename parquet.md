
See parquet_file/src/metadata.rs for details on the flow of the data

Convert the given metadata and RecordBatches to parquet file bytes. Used by `ingester`.

parquet_file/src/storage.rs

pub async fn parquet_bytes(

##### Persist compacted data to parquet files in object storage

At top level:

```rust
rg 'persist\('
```

ingester/src/persist.rs

```rust
cd ingester
cnr persist
```

### where is parquet referenced in iox

* compactor
* ingester

* generated_types

* influxdb_iox_client
* influxdb_iox/src/commands
* influxdb_iox/tests

* iox_catalog
* iox_data_generator

* parquet_file
* querier
* query_tests

* service_grpc_catalog
* service_grpc_object_store

* docs/testing.md
