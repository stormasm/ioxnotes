
To see where object_store is happening do a

```rust
rg DynObjectStore
```

#### Relevant crates for object_store

* clap_blocks - *object_store.rs*
* compactor2
* garbage_collector - *tool to clean up old object store files that don't appear in the catalog*
* parquet_file - *storage.rs*

#### LocalFileSystem

clap_blocks/src/object_store.rs

There are some details in here for setting up a local filesystem object store.
