
Start up Zookeeper
```rust
zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties
```

Start up Kafka
```rust
kafka-server-start /usr/local/etc/kafka/server.properties
```

To run tests:

```rust
TEST_INTEGRATION=1 TEST_BROKER_IMPL=kafka KAFKA_CONNECT=localhost:9092 cargo test
TEST_INTEGRATION=1 TEST_BROKER_IMPL=kafka KAFKA_CONNECT=localhost:9092 cargo test test_topic_crud
```

```rust
alias szoo='zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties'
alias skafka='kafka-server-start /usr/local/etc/kafka/server.properties'

alias rsk='06; cd rskafka'
alias tk0='rsk; TEST_INTEGRATION=1 TEST_BROKER_IMPL=kafka KAFKA_CONNECT=localhost:9092 cargo test'
alias tk1='rsk; TEST_INTEGRATION=1 TEST_BROKER_IMPL=kafka KAFKA_CONNECT=localhost:9092 cargo test test_topic_crud'
```

### References

[installing kafka on homebrew](https://medium.com/@Ankitthakur/apache-kafka-installation-on-mac-using-homebrew-a367cdefd273)

