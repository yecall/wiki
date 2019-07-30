
# PoC-3 Release Notes

## Table of Contents

- [Description](#description)
    - [Feture list](##feature-list)
    - [Limitations and future works](##limitations-and-future-work)
- [How to engage](#how-to-engage)
    - [As wallet users](#as-wallet-user)
    - [As fullnode maintainers](#as-fullnode-maintainer)
- [Feedback](#feedback)

## Description

PoC-3 is described in the [roadmap section](https://github.com/yeeco/yeeroot#roadmap) of yeeroot project description:

>PoC-3: PoW consensus, static sharding (2019-07)

### Feature list
1. PoW consensus

    The block head has been already designed for multi mining.
        
    Block interval: 60s
    
1. Static full sharding

    The blockchain was initilized with **4 shards**, 
    which means the workloads of network, transactions, executions, 
    storage are split into 4 independent and parallel parts. 
 
### Limitations and future works
 
1. cross shard transaction is not supported, will be implemented in PoC-4. 
2. Block interval is not short enough, will be improved in PoC-5.

## How to engage

### As wallet users
If you just want to try with token transfer, you can visit [poctest page](http://pocnet.yeeco.io),
where you can create wallet, check balance and make transfers.

### As fullnode maintainers
If you want to run a full node and play it by RPC, you can follow the steps.
```
TODO
``` 

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
