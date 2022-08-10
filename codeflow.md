
### Router

router/src/lib.rs

An IOx router is responsible for:

* Creating IOx namespaces & synchronising them within the catalog.
* Handling writes:
* Receiving IOx write/delete requests via HTTP and gRPC endpoints.
* Enforcing schema validation & synchronising it within the catalog.
* Deriving the partition key of each DML operation.
* Applying sharding logic.
* Push resulting operations into the appropriate kafka topics.


### There are two paths for writes

* router::server::grpc
* router::server::http

### This is what happens with the iox write

```rust
alias ioxwtemp='ioxg; iox write postgresql:///iox_shared ./test_fixtures/lineproto/temperature.lp --host http://localhost:8081'
alias ioxwtemplums='ioxg; iox write plums ./test_fixtures/lineproto/temperature.lp --host http://localhost:8081'
```

```rust
2022-08-10T16:50:38.143434Z DEBUG router::server::grpc: routing grpc write num_tables=1 namespace=postgresql:///iox_shared
```

### This is what happens with the low_card data

```rust
2022-08-10T16:34:15.550988Z DEBUG ioxd_common::http: Processing request request=Request { method: POST, uri: /api/v2/write?org=26f7e5a4b7be365b&bucket=917b97a92e883afc, version: HTTP/1.1, headers: {"accept": "*/*", "host": "localhost:8080", "content-length": "930"}, body: Body(Streaming) }

2022-08-10T16:34:15.558328Z DEBUG router::server::http: routing write num_lines=10 num_fields=10 num_tables=10 precision=Nanoseconds body_size=930 namespace=26f7e5a4b7be365b_917b97a92e883afc org=26f7e5a4b7be365b bucket=917b97a92e883afc

2022-08-10T16:34:15.580738Z DEBUG write_summary: Creating write summary metas=[[DmlMeta { sequence: Some(Sequence { sequencer_id: 0, sequence_number: SequenceNumber(6) }), producer_ts: Some(2022-08-10T16:34:15.566325+00:00), span_ctx: None, bytes_read: Some(3752) }]]

2022-08-10T16:34:15.582456Z DEBUG ioxd_common::http: Successfully processed request response=Response { status: 204, version: HTTP/1.1, headers: {"x-iox-write-token": "eyJzZXF1ZW5jZXJzIjpbeyJzZXF1ZW5jZU51bWJlcnMiOlsiNiJdfV19"}, body: Body(Empty) }

2022-08-10T16:34:15.585254Z DEBUG ioxd_common::http: Processing request request=Request { method: POST, uri: /api/v2/write?org=26f7e5a4b7be365b&bucket=917b97a92e883afc, version: HTTP/1.1, headers: {"accept": "*/*", "host": "localhost:8080", "content-length": "940"}, body: Body(Streaming) }

2022-08-10T16:34:15.590312Z DEBUG router::server::http: routing write num_lines=10 num_fields=10 num_tables=10 precision=Nanoseconds body_size=940 namespace=26f7e5a4b7be365b_917b97a92e883afc org=26f7e5a4b7be365b bucket=917b97a92e883afc

2022-08-10T16:34:15.598801Z DEBUG write_summary: Creating write summary metas=[[DmlMeta { sequence: Some(Sequence { sequencer_id: 0, sequence_number: SequenceNumber(7) }), producer_ts: Some(2022-08-10T16:34:15.594217+00:00), span_ctx: None, bytes_read: Some(3752) }]]

2022-08-10T16:34:15.599165Z DEBUG ioxd_common::http: Successfully processed request response=Response { status: 204, version: HTTP/1.1, headers: {"x-iox-write-token": "eyJzZXF1ZW5jZXJzIjpbeyJzZXF1ZW5jZU51bWJlcnMiOlsiNyJdfV19"}, body: Body(Empty) }
```


### This is how the data initially gets into Iox

What is DML in database?

DML is an abbreviation for Data Manipulation Language. Represents a collection of programming languages explicitly used to make changes to the database, such as: CRUD operations to create, read, update and delete data. Using INSERT, SELECT, UPDATE, and DELETE commands.

https://github.com/influxdata/influxdb_iox/pull/3084

As part of routing deletes through the write buffer, I need to define an in-memory type for the data that is being sequenced. Following internal discussion we decided to call this DML to be consistent with other database's name for this.

It didn't seem write to define these types in mutable_batch as it has no concept of a delete, but it also seemed unfortunate to define them in write_buffer and therefore force unrelated code to depend on things like rdkafka. I therefore lifted and renamed WriteMeta and DbWrite into a new crate called dml