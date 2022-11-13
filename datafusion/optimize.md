
To turn off the optimizer code there are two ways to do it...

* core/src/execution/context.rs

```rust
/// Creates a physical plan from a logical plan.
 pub async fn create_physical_plan(
     &self,
     logical_plan: &LogicalPlan,
 ) -> Result<Arc<dyn ExecutionPlan>> {
     let planner = self.query_planner.clone();
     // comment out this line to turn off optimization step
     //let logical_plan = self.optimize(logical_plan)?;
     planner.create_physical_plan(&logical_plan, self).await
 }
```

* optimizer/src/optimizer.rs

```rust
/// Log the plan in debug/tracing mode after some part of the optimizer runs
fn log_plan(description: &str, plan: &LogicalPlan) {
    // comment out next line to turn off debug! output
    //debug!("{description}:\n{}\n", plan.display_indent());
    trace!("{description}::\n{}\n", plan.display_indent_schema());
}
```
