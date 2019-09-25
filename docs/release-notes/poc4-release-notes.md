
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

### As full node maintainers
If you want to run a full node and play it by RPC, you can follow the steps.

1. Download [official docker image](https://hub.docker.com/r/yeeco/yeeroot) yeeco/yeeroot:20190924
1. Start docker container, with a picked shard number
    ```
    docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666
    ``` 

1. If you'd like to join in POW mining.
    
    1. Way 1: single mining
    
        ```
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coin-base=YourCoinBaseAddressHere --mine
        ```
             
    1. Way 2: multi mining
    
        1 ) Start 4 nodes of the 4 shards
        ```
        mkdir -p ./data/yee
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee --shard-num=0 --rpc_port=9933 --ws_port=9944 --port=30333 --foreign_port=30334 --bath-path=/data/yee/shard_0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coin-base=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee --shard-num=1 --rpc_port=19933 --ws_port=19944 --port=31333 --foreign_port=31334 --bath-path=/data/yee/shard_1 --bootnodes-routers=http://3.1.169.4:6666 --validator --coin-base=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee --shard-num=2 --rpc_port=29933 --ws_port=29944 --port=32333 --foreign_port=32334 --bath-path=/data/yee/shard_2 --bootnodes-routers=http://3.1.169.4:6666 --validator --coin-base=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee --shard-num=3 --rpc_port=39933 --ws_port=39944 --port=33333 --foreign_port=33334 --bath-path=/data/yee/shard_3 --bootnodes-routers=http://3.1.169.4:6666 --validator --coin-base=YourCoinBaseAddressHere
        ```
   
        2 ) Prepare switch (works as a multi-miner) configuration file
        
        file path: ./data/yee/conf/switch.toml
        
        file content: 
        ```
        [shards]
        [shards.0]
        rpc = ["http://127.0.0.1:9933"]
        
        [shards.1]
        rpc = ["http://127.0.0.1:19933"]
        
        [shards.2]
        rpc = ["http://127.0.0.1:29933"]
        
        [shards.3]
        rpc = ["http://127.0.0.1:39933"]
        ```
        
        3 ) Start switch
        ```
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20190924 yee switch --bath-path=/data/yee --mine
        ```

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
