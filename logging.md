Iox with logging, note the run all-in-one is needed for logging to work

```rust
alias ioxinfo='iox run all-in-one --log-filter info'
alias ioxdebug='iox run all-in-one --log-filter debug'
```

##### Turn off debug logging in noisy compactor

```rust
ingester/src/stream_handler/periodic_watermark_fetcher.rs
147: debug!("fetching write buffer watermark");

compactor/src/handler.rs
177: debug!(n_candidates, "found compaction candidates");

compactor/src/compact.rs
342: debug!(
    candidate_num = candidates.len(),
    "Number of candidate partitions to be considered to compact"
);
```

For more details on logging

[Contributing](https://github.com/influxdata/influxdb_iox/blob/main/CONTRIBUTING.md)

* specifically talks about issues with h2 (more research needed)
