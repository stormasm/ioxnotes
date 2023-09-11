
This project is the integration of Nushell into Iox.  In other words Nushell
is now embedded inside Iox and works out of the box.

The motivation for this project is to provide moving forward a much richer interactive experience for the Iox end user by enhancing the terminal user experience for
Iox developers and users by way of the advanced nushell language and architecture.

At the moment the only real interaction an end user has with Iox is via the
* IOx interactive SQL REPL loop
* functionality provided by the command 

```rust
influxdb_iox help
```

This project covers almost everything available in the SQL Repl Loop along
with the ability to write both strings and files of
[Line protocol data](https://docs.influxdata.com/influxdb/v2.3/reference/syntax/line-protocol/)
similar to the command

```rust
influxdb_iox write
```

To get up and running with Iox and Nushell there are several steps you need
to go through.

1. [Build Iox](#build-iox)
1. [Build Nushell Commands](#build-nushell-commands)
1. [Build Nushell](#build-nushell)
1. [Iox Nushell Command Set](#nushell-commands)

### Build Iox

This document assumes you are familiar with building Iox from source and have used it prior to launching down this path.  If you are not familiar with building and using Iox then you should start here:
1. [Getting Started](https://github.com/influxdata/influxdb_iox#get-started)
1. [IOx Catalog](https://github.com/influxdata/influxdb_iox/tree/main/iox_catalog)

### Build Nushell Commands

The Nushell Command set is extensive.  For this first version of code we included all of the nushell commands so that all of its functionality will be
available to the end user of Iox.  In the future, we could reduce the size of the default nushell commands and only build and install the relevant features
to working with Iox.

```rust
cd nu_iox
cargo build
```

Now that these commands are built one can move forward to build Nushell.

### Build Nushell

To build Nushell:

```rust
cd iox_nu
cargo build
```

To run Nushell

```
./target/debug/nu
```

### Nushell Commands

To see all of the Iox Nushell Commands simply type

```rust
help --find iox
```

You should see four commands listed if everything is working

* ioxnamespace
* ioxsql
* ioxwrite
* ioxwritefile

To see help on an individual command 

```rust
ioxnamespace --help
ioxsql --help
ioxwrite --help
ioxwritefile --help
```

Set a default database that all of the commands will use unless specifically noted via the -d flag

All of the above commands except for **ioxnamespace** reference the default database via the environment variable called **IOX_DBNAME**

If you ever want to change the default database that nushell references when running commands simply change it using the **let-env** command


```rust
let-env IOX_DBNAME = "bananas"
```

which is similar to the use command in the sql repl

```rust
use bananas;
```

## Tutorial

* [ioxwrite](#ioxwrite)
* [ioxwritefile](#ioxwritefile)
* [ioxnamespace](#ioxnamespace)
* [ioxsql](#ioxsql)

Start out by setting 

```rust
export IOX_DBNAME='plums'
```

In one of your shells or config files.  This will set the default database name...

### ioxwrite

Now write some data to the iox database.

```rust
ioxwrite "h2o_temperature,location=puget_sound,state=WA surface_degrees=53.7,bottom_degrees=42.1 16007456160"
```

### ioxwritefile

```rust
cd influxdb_iox/test_fixtures/lineproto
ioxwritefile ./temperature.lp
```

### ioxnamespace

Get the names of all of the Iox databases

```rust
ioxnamespace
```

### ioxsql

show all of the tables in the database

```rust
ioxsql "show tables"
```

show the columns in the h2o_temperature table

```rust
ioxsql "show columns from h2o_temperature"
```

Query the database for the table h2o_temperature

```rust
ioxsql "select * from h2o_temperature"
```

### let-env

Change the name of the default database

```rust
let-env IOX_DBNAME = "peaches"
```

and run through all of the commands from above again.

and then test it by switching databases back to the default which is plums
and see how the data changes accordingly...

7/31/2022
Sunday
