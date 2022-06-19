
### How to suppress logging

##### compactor/src/compact.rs

turn off debug! for
* Number of candidate partitions to be considered to compact

##### compactor/src/handler.rs

turn off
* debug!(n_candidates, "found compaction candidates");

##### ingester/src/stream_handler/periodic_watermark_fetcher.rs

turn off
* debug!("fetching write buffer watermark");
