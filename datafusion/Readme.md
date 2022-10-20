
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