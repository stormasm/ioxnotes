
To run the iox subcommands the server must be up first

```rust
iox
ioxrun sql
```

### Postgresql Notes

[catalog notes](https://github.com/influxdata/influxdb_iox/tree/main/iox_catalog)

If *sqlx-cli* is not installed go ahead and do it...

```rust
cargo install sqlx-cli
```

Then go ahead and create the database

```rust
DATABASE_URL=postgres:///iox_shared sqlx database create
```

Run the following two commands at the top level influxdb_iox

```rust
cargo run -q -- catalog setup
cargo run -- catalog topic update iox-shared
```

Then cd iox_catalog and run

```rust
cargo test or cnr
```


### References
* [youtube](https://github.com/stormasm/youtube/blob/main/cs/iox.md)
* [slack](./iox-slack.md)
* [postgresql](./postgresql.md)
* [gdoc](https://docs.google.com/document/d/e/2PACX-1vQ2cTRWln8Nyn2yQh4CWwvyN5yI3Op-4RA3MpB1moivlQtBBQ9KhR5N2vvkMiZz51t_1EPhj5SDmJh5/pub)
