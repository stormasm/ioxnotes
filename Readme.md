
### To get up and running

```rust
iox
iox01
iox02
```

### Aliases

```rust
alias ioxg='cd ~/j/tmp06/influxdb_iox'
alias iox='ioxg; ./target/debug/influxdb_iox'
alias iox01='iox -vv write company_sensors test_fixtures/lineproto/metrics.lp --host http://localhost:8080'
alias iox02='iox query company_sensors "SELECT * FROM cpu LIMIT 10"'
```

### Historical Note

Start of new instructions in late summer 2023.  I have not been using iox for awhile and we now have a local sqlite db. And postgresql is no longer required to get up and running.  For more details see [PR 7027](https://github.com/influxdata/influxdb_iox/pull/7027).
