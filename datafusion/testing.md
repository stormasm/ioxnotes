

core/tests/user_defined_plan.rs
```rust
06; cd dfcli-data; dfcli
CREATE EXTERNAL TABLE sales(customer_id VARCHAR, revenue BIGINT) STORED AS CSV location 'customer.csv';
explain SELECT customer_id, revenue FROM sales ORDER BY revenue DESC limit 3;
```