
For more info on testing
[read this doc first](https://github.com/influxdata/influxdb_iox/blob/main/docs/testing.md)

So one way to do end to end testing is simply to run this command from the top level...

```rust
ioxg
ct --test end_to_end
```

Or you can

```rust
alias ioxioxt='ioxg; cd influxdb_iox/tests/end_to_end_cases'
ct
```
