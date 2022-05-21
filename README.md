
```rust
cp influxdb_iox <directory in your path>
alias iox='influxdb_iox'
```

check to make sure if the database iox_shared exists in psql
and if it does not go ahead and create it.

```rust
psql -l
DATABASE_URL=postgres:///iox_shared sqlx database create
psql -l
```

then you can bring up iox with this command

```rust
./target/debug/influxdb_iox run all-in-one --catalog-dsn postgres:///iox_shared --data-dir=~/iox_data
```

[or] you can bring up iox with this command

```rust
iox --catalog-dsn postgres:///iox_shared --data-dir=/Users/ma/j/tmp06/iox_data
```

and write data out with this command

```rust
cargo run -- write postgresql:///iox_shared ./test_fixtures/lineproto/metrics.lp --host http://localhost:8081
```

[or] write data out with this command

```rust
iox write postgresql:///iox_shared ./test_fixtures/lineproto/metrics.lp --host http://localhost:8081
```
