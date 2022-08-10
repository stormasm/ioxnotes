
### This is how the data initially gets into Iox

What is DML in database?

DML is an abbreviation for Data Manipulation Language. Represents a collection of programming languages explicitly used to make changes to the database, such as: CRUD operations to create, read, update and delete data. Using INSERT, SELECT, UPDATE, and DELETE commands.

https://github.com/influxdata/influxdb_iox/pull/3084

As part of routing deletes through the write buffer, I need to define an in-memory type for the data that is being sequenced. Following internal discussion we decided to call this DML to be consistent with other database's name for this.

It didn't seem write to define these types in mutable_batch as it has no concept of a delete, but it also seemed unfortunate to define them in write_buffer and therefore force unrelated code to depend on things like rdkafka. I therefore lifted and renamed WriteMeta and DbWrite into a new crate called dml