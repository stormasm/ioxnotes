
See the Cargo.toml files to see what crates from iox get added to nushell so that the influxdb_iox_client fires up correctly...

Note forks exist in these spots

 * [angermanmichael](https://github.com/angermanmichael/nushell)
 
 These are the crates from iox that get added to nushell
 
 	"crates/arrow_util",  
	"crates/client_util",  
	"crates/data_types",  
	"crates/datafusion",  
	"crates/datafusion_util",  
	
	"crates/dml",  
	"crates/generated_types",  
	"crates/influxdb_iox_client",  
	"crates/influxdb_line_protocol",  
	"crates/iox_time",  
	
	"crates/mutable_batch",  
	"crates/mutable_batch_lp",  
	"crates/mutable_batch_pb",  
	"crates/observability_deps",  
	"crates/predicate",  
	
	"crates/query_functions",  
	"crates/schema",  
	"crates/test_helpers",  
	"crates/trace",  
	"crates/workspace-hack",  