
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
brew update
brew install postgresql@15
brew services start postgresql@15
brew services restart postgresql@15
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

### After Installing with Brew here are the notes

This formula has created a default database cluster with:
  initdb --locale=C -E UTF-8 /opt/homebrew/var/postgresql@15
For more details, read:
  https://www.postgresql.org/docs/15/app-initdb.html

postgresql@15 is keg-only, which means it was not symlinked into /opt/homebrew,
because this is an alternate version of another formula.

If you need to have postgresql@15 first in your PATH, run:
  echo 'export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc

For compilers to find postgresql@15 you may need to set:
  export LDFLAGS="-L/opt/homebrew/opt/postgresql@15/lib"
  export CPPFLAGS="-I/opt/homebrew/opt/postgresql@15/include"


To start postgresql@15 now and restart at login:
  brew services start postgresql@15
==> Summary
🍺  /opt/homebrew/Cellar/postgresql@15/15.2: 3,348 files, 45.7MB
==> Running `brew cleanup postgresql@15`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).
==> Caveats
==> postgresql@15
This formula has created a default database cluster with:
  initdb --locale=C -E UTF-8 /opt/homebrew/var/postgresql@15
For more details, read:
  https://www.postgresql.org/docs/15/app-initdb.html

postgresql@15 is keg-only, which means it was not symlinked into /opt/homebrew,
because this is an alternate version of another formula.

If you need to have postgresql@15 first in your PATH, run:
  echo 'export PATH="/opt/homebrew/opt/postgresql@15/bin:$PATH"' >> ~/.zshrc

For compilers to find postgresql@15 you may need to set:
  export LDFLAGS="-L/opt/homebrew/opt/postgresql@15/lib"
  export CPPFLAGS="-I/opt/homebrew/opt/postgresql@15/include"

To start postgresql@15 now and restart at login:
  brew services start postgresql@15
