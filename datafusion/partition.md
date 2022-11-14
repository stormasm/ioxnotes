
* physical_plan/mod.rs

```rust
// Hard code target_partitions as it appears in the RepartitionExec output
let config = SessionConfig::new().with_target_partitions(1);
let ctx = SessionContext::with_config(config);
```

* along with this code as well...

```rust
pub enum Partitioning
```