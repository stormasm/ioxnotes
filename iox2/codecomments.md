




https://github.com/influxdata/influxdb_iox/pull/6228

Adds an ingester2 crate to hold the MVP of the Kafkaless project.

This was necessary due to the tight coupling of the ingester internals
with tests in external crates, and eases the parallel development of two
version of the ingester.

This commit contains various changes from the "ingester" crate, mostly
removing the concept/references to a "shard" or "ShardId" where
possible.

This commit does not copy over all of the "ingester" crate - only those
components that are definitely needed. I will drag across more as
functionality is implemented.