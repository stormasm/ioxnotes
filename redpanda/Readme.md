
To run the redpanda console

    docker compose up

### rpk commands

* rpk container start
* rpk container stop
* rpk container purge

### After brew install

You can start a 3 node cluster locally using the following command: 

    rpk container start -n 3

You can then interact with the cluster using commands like the following: 

    rpk topic list

When done, you can stop and delete the cluster with the following command:
    
    rpk container purge

For information on how to setup production evironments, check out our
installation guide here: https://vectorized.io/documentation/setup-guide/

### After Starting

Downloading latest version of Redpanda   
Starting cluster   
Waiting for the cluster to be ready...   

```rust
  NODE ID  ADDRESS          
  0        127.0.0.1:57267  
  2        127.0.0.1:57268  
  1        127.0.0.1:57263  
```

Cluster started! You may use rpk to interact with it. E.g:   

  rpk cluster info --brokers 127.0.0.1:57267,127.0.0.1:57268,127.0.0.1:57263

You may also set an environment variable with the comma-separated list of broker addresses:

```rust
  export REDPANDA_BROKERS="127.0.0.1:57267,127.0.0.1:57268,127.0.0.1:57263"
  rpk cluster info
```