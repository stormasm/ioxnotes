
check to make sure if the database iox_shared exists in psql
and if it does not go ahead and create it.

```rust
psql -l
DATABASE_URL=postgres:///iox_shared sqlx database create
psql -l

then you can bring up iox with this command

```rust
./target/debug/influxdb_iox run all-in-one --catalog-dsn postgres:///iox_shared --data-dir=~/iox_data
```
