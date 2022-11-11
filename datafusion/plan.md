
### ex04.rs: select a, b from example order by b desc

```rust
logical_plan: 
Sort: example.b DESC NULLS FIRST   
  Projection: example.a, example.b  
    TableScan: example projection=[a, b]   
physical plan:
SortExec: [b@1 DESC]    
  ProjectionExec: expr=[a@0 as a, b@1 as b]     
    CsvExec: files=[Users/ma/j/tmp06/rust-examples/datafusion/data/red0.csv],
               has_header=true, limit=None, projection=[a, b]
```

### ex05.rs: Select a, f from example where f > 15

```rust
logical plan:
Projection: example.a, example.f    
  Filter: example.f > Int64(15)   
    TableScan: example projection=[a, f], partial_filters=[example.f > Int64(15)] 
physical plan:
ProjectionExec: expr=[a@0 as a, f@1 as f]     
  FilterExec: f@1 > 15    
    CsvExec: files=[Users/ma/j/tmp06/rust-examples/datafusion/data/red0.csv],
               has_header=true, limit=None, projection=[a, f] 
```

[For more details](https://github.com/stormasm/rust-examples/tree/main/datafusion)

### References

https://www.sqlitetutorial.net/
