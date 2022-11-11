
### Select a, b from example order by b desc

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