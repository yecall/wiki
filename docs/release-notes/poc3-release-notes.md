
# YeeCo PoC-3 Release Notes

2019-07-30

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
If you just want to try with token transfer, you can visit [poctest page](https://pocnet.yeeco.io),
where you can create wallet, check balance and make transfers.

### As fullnode maintainers
If you want to run a full node and play it by RPC, you can follow the steps.

1. Download [official docker image](https://hub.docker.com/r/yeeco/yeeroot) yeeco/yeeroot:20190730
2. Start docker container, with a picked shard number
```
docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190730 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666
``` 
3. If you'd like to join in POW mining, append `--validator --coin-base=YourCoinBaseAddressHere` to the docker command line

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
