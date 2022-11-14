
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

physical_plan/mod.rs
```rust
    pub async fn collect
```

physical_plan/common.rs
```rust
    pub async fn collect
```