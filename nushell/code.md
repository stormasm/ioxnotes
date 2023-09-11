
Inside nu-command is *nuclient.rs* which is a copy of *repl.rs* without a few of the pieces

```rust
pub struct Repl {
    /// Client for interacting with IOx namespace API
    namespace_client: influxdb_iox_client::namespace::Client,

    /// Client for running sql
    flight_client: influxdb_iox_client::flight::Client,

    /// namespace name against which SQL commands are run
    query_engine: Option<QueryEngine>,

    /// Formatter to use to format query results
    output_format: QueryOutputFormat,
}
```
