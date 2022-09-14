
[How to completely uninstall and reinstall Homebrew Postgres](https://blog.testdouble.com/posts/2021-01-28-how-to-completely-uninstall-homebrew-postgres/)

```rust
$ brew uninstall postgres
$ rm -rf /usr/local/var/postgres
$ rm /usr/local/var/log/postgres.log
$ rm -f ~/.psqlrc ~/.psql_history
```

double triple check for sure, you do not have to do this:

```rust
brew list --formula | grep -e postgres -e psql
sudo find / -name "*postgres*" -o -name "*psql*"
```

```rust
brew update
brew doctor
```

```rust
brew install postgres
brew services start postgresql
```

#### Ref
* [startup](./startup.md)
* [gdoc](https://docs.google.com/document/d/e/2PACX-1vQ2cTRWln8Nyn2yQh4CWwvyN5yI3Op-4RA3MpB1moivlQtBBQ9KhR5N2vvkMiZz51t_1EPhj5SDmJh5/pub)

### Other notes

From this file: iox_catalog/src/postgres.rs

```rust
async fn test_billing_summary_on_parqet_file_creation() {
    // If running an integration test on your laptop, this requires that you have Postgres running
    //
    // This is a command to run this test on your laptop
    //    TEST_INTEGRATION=1 TEST_INFLUXDB_IOX_CATALOG_DSN=postgres:postgres://$USER@localhost/iox_shared RUST_BACKTRACE=1 cargo test --package iox_catalog --lib -- postgres::tests::test_billing_summary_on_parqet_file_creation --exact --nocapture
    //
    // If you do not have Postgres's iox_shared db, here are commands to install Postgres (on mac) and create iox_shared db
    //    brew install postgresql
    //    initdb pg
    //    createdb iox_shared
```