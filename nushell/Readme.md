
Steps to integrate nushell into iox.

* Modify the top level iox Cargo.toml file and add the crate **iox_nu**

### The iox_nu crate

* Grab a copy of the nushell released src code
* Get the docs and src directories from top level nushell and put them in the iox_nu crate.
* Grab the Cargo.toml file from top level nushell and **modify accordingly** similar to a previous version of this code.
* Copy over the Readme.md file from a previous version of this code.

### The nu_iox crate

See the Cargo.toml files to see what crates from iox get added to nushell so that the influxdb_iox_client fires up correctly...
 
 These are the crates from iox that get added to nushell.  You will note at the moment the number is 20 crates.
 
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