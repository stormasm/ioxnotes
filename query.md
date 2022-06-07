
Bring up iox
```rust
iox
```

Write out the temp data...
```rust
alias ioxwtemp='ioxg; iox write postgresql:///iox_shared ./test_fixtures/lineproto/temperature.lp --host http://localhost:8081'
```

Bring up the sql client
```rust
iox sql
```

Now you are in the sql client.

* use postgresql:///iox_shared;
* show namespaces;
* show tables;
* select * from h2o_temperature;
* select * from h2o_temperature where state = 'WA';
* select * from h2o_temperature where state = 'CA';

## Code related to querying

##### test_helpers_end_to_end/src/client.rs

* try_run_query

##### influxdb_iox_client/src/client/flight.rs

* perform_query
