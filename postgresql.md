
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
* [iox](./iox.md)
* [gdoc](https://docs.google.com/document/d/e/2PACX-1vQ2cTRWln8Nyn2yQh4CWwvyN5yI3Op-4RA3MpB1moivlQtBBQ9KhR5N2vvkMiZz51t_1EPhj5SDmJh5/pub)
