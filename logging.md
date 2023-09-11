
##### Turn off debug logging in noisy compactor

This was closed out with my [PR](https://github.com/influxdata/influxdb_iox/pull/8684)

*compactor/src/components/compaction_jobs_source/logging.rs*

make this a trace! statement

```rust
async fn fetch(&self) -> Vec<CompactionJob> {
    let jobs = self.inner.fetch().await;
    info!(n_jobs = jobs.len(), "Fetch jobs",);
    if jobs.is_empty() {
        trace!("No compaction job found");
    }
    jobs
}
```

### nushell std-lib bug

Influxdb was crashing inside nushell when the std-lib was turned on...

[see this issue](https://github.com/nushell/nushell/issues/10248)

this was proven out by changing

```rust
LOG_FORMAT to IOX_LOG_FORMAT
```

inside *trogging/src/cli.rs*

then iox came up fine with no issues

### All-in-one

Iox with logging, note the run all-in-one is needed for logging to work

```rust
alias ioxinfo='iox run all-in-one --log-filter info'
alias ioxdebug='iox run all-in-one --log-filter debug'

### No h2 logging

alias ioxdebugnoh2='iox run all-in-one --log-filter debug,hyper::proto::h1=info,h2=info'
```

For more details on logging

[Contributing](https://github.com/influxdata/influxdb_iox/blob/main/CONTRIBUTING.md)

* specifically talks about issues with h2 (more research needed)
