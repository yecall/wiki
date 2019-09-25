
# YeeCo PoC-4 Release Notes

2019-09-25

## Table of Contents

- [Description](#description)
    - [Feture list](##feature-list)
    - [Limitations and future works](##limitations-and-future-work)
- [How to engage](#how-to-engage)
    - [As wallet users](#as-wallet-user)
    - [As fullnode maintainers](#as-fullnode-maintainer)
- [Feedback](#feedback)

## Description

PoC-4 is described in the [roadmap section](https://github.com/yeeco/yeeroot#roadmap) of yeeroot project description:

>PoC-4: Multi-mining, cross-shard transactions (2019-09)

### Feature list
1. Multi-mining

    The yeeroot node provides 2 RPCs for multi-mining: 
    
    [mining_getJob](https://github.com/yeeco/wiki/wiki/RPC%20Description#mining_getJob)
    
    [mining_submitJob](https://github.com/yeeco/wiki/wiki/RPC-Description#mining_submitJob)
        
    The yeeroot switch can work as a multi-mimer
    
1. Cross shard transactions

    The yeeroot node supports cross shard transactions, 
    which means that you can transfer coins from an address on one shard to an address on another shard.
 
1. [WIP] CRFG
    
    The yeeroot node can make consensus on block finalization by the voters initiated in the genesis block.

 
### Limitations and future works
 
1. Strictly speaking，the relay transaction of a cross shard transaction relies on the finalization the origin transaction.
   In current PoC version, we implemented it in a simple way: we treated an origin transaction as finalized when it was 2-confirmed.
   The full implementation with CRFG will be released in PoC-5.

1. The votes of CRFG should be recent block miners. Full CRFG version will be released in PoC-5.

1. Block interval is not short enough, will be improved in PoC-5.

## How to engage

### As wallet users
If you just want to try with token transfer, you can visit [poctnet page](https://pocnet.yeeco.io),
where you can create wallet, check balance and make transfers.

### As fullnode maintainers
If you want to run a full node and play it by RPC, you can follow the steps.

1. Download [official docker image](https://hub.docker.com/r/yeeco/yeeroot) yeeco/yeeroot:20190730
2. Start docker container, with a picked shard number
```
docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190730 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666
``` 

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
