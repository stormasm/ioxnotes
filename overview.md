IOx router role implementation.

#### An IOx router is responsible for:

* Creating IOx namespaces & synchronising them within the catalog.
* Handling writes:
* Receiving IOx write/delete requests via HTTP and gRPC endpoints.
* Enforcing schema validation & synchronising it within the catalog.
* Deriving the partition key of each DML operation.
* Applying sharding logic.
* Push resulting operations into the appropriate kafka topics.
