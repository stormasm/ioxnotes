
So the goal here is to learn about how the catalog works so that we can port the catalog over from postgresql to sqlite.

### Who is a user of the iox_catalog ?

* ingester (mainly in data.rs)

##### References

iox_catalog is referenced in these crates
```rust
rg iox_catalog -g Cargo.toml
```

* data_types
* sqlx-hotswap-pool
* test_helpers_end_to_end
