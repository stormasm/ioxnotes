
### Setup of Datafusion including testing

* [setup.md](./setup.md)

### Performance of Polars versus Datafusion

[Dataframe Showdown: Polars versus Datafusion](https://www.confessionsofadataguy.com/dataframe-showdown-polars-vs-spark-vs-pandas-vs-datafusion-guess-who-wins/)

### Slack: Arrow-Rust

* channel started on March 10, 2021
* [First Message](https://the-asf.slack.com/archives/C01QUFS30TD/p1615401105000200)

# Move everything below here somewhere else

### planner.rs : aggregate_fn_to_expr

* sql_statement_to_plan
* query_to_plan
* query_to_plan_with_alias
* set_expr_to_plan
* select_to_plan
* sql_expr_to_logical_expr
* aggregate_fn_to_expr

### How TableProvider works

```rust
rg 'impl TableProvider'
rg create_physical_plan
```

in datafusion/core/src

### How collect works

```rust
rg "pub async fn collect"
```

dataframe.rs
```rust
impl DataFrame
    pub async fn collect(&self)
```

The collect inside dataframe.rs calls create_physical_plan which calls into the SessionStates create_physical_plan which creates a physical plan from a logical plan that is defined inside the dataframe.

This physical plan which just got created then calls the collect inside physical_plan/mod.rs which calls execute_stream inside physical_plan/mod.rs which calls execute on the ExecutionPlan...

physical_plan/mod.rs
```rust
    pub async fn collect
```

physical_plan/common.rs
```rust
    pub async fn collect
```

