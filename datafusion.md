
* iox_query/src/lib.rs

```rust
/// Provides access to raw `QueryChunk` data as an
/// asynchronous stream of `RecordBatch`es filtered by a *required*
/// predicate. Note that not all chunks can evaluate all types of
/// predicates and this function will return an error
/// if requested to evaluate with a predicate that is not supported
///
/// This is the analog of the `TableProvider` in DataFusion
///
/// The reason we can't simply use the `TableProvider` trait
/// directly is that the data for a particular Table lives in
/// several chunks within a partition, so there needs to be an
/// implementation of `TableProvider` that stitches together the
/// streams from several different `QueryChunk`s.

fn read_filter(
    &self,
    ctx: IOxSessionContext,
    predicate: &Predicate,
    selection: Selection<'_>,
) -> Result<SendableRecordBatchStream, QueryChunkError>;

/// Returns chunk type which is either MUB, RUB, OS
fn chunk_type(&self) -> &str;

/// Order of this chunk relative to other overlapping chunks.
fn order(&self) -> ChunkOrder;
}
```

* core/src/datasource/datasource.rs

```rust
/// Source table
#[async_trait]
pub trait TableProvider: Sync + Send {
    /// Returns the table provider as [`Any`](std::any::Any) so that it can be
    /// downcast to a specific implementation.
    fn as_any(&self) -> &dyn Any;

    /// Get a reference to the schema for this table
    fn schema(&self) -> SchemaRef;

    /// Get the type of this table for metadata/catalog purposes.
    fn table_type(&self) -> TableType;

    /// Create an ExecutionPlan that will scan the table.
    /// The table provider will be usually responsible of grouping
    /// the source data into partitions that can be efficiently
    /// parallelized or distributed.
    async fn scan(
        &self,
        ctx: &SessionState,
        projection: &Option<Vec<usize>>,
        filters: &[Expr],
        // limit can be used to reduce the amount scanned
        // from the datasource as a performance optimization.
        // If set, it contains the amount of rows needed by the `LogicalPlan`,
        // The datasource should return *at least* this number of rows if available.
        limit: Option<usize>,
    ) -> Result<Arc<dyn ExecutionPlan>>;

    /// Tests whether the table provider can make use of a filter expression
    /// to optimise data retrieval.
    fn supports_filter_pushdown(
        &self,
        _filter: &Expr,
    ) -> Result<TableProviderFilterPushDown> {
        Ok(TableProviderFilterPushDown::Unsupported)
    }
}
```
