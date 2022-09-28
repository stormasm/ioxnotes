
### write

```rust
iox --host=http://localhost:8080 write my_namespace test_fixtures/lineproto/air_and_water.lp 
```

### query

```rust
iox query my_namespace 'show tables'
```

### debug

```rust
iox debug

iox debug print-cpu

iox debug namespace list

iox debug schema get [low_card]

iox debug parquet-to-lp [/Users/ma/j/tmp22/iox_datadir/1/1/1/1/c7dd62ab-7796-48ad-81d6-f3fecd3a979f.parquet] 

alias ioxpartolp='iox debug parquet-to-lp'
```

### aliases

```rust
alias iox='ioxg; ./target/debug/influxdb_iox'
```
