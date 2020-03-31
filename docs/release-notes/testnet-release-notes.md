
# YeeCo Testnet Release Notes

2019-03-30

## Table of Contents

- [Description](#description)
    - [Feture list](##feature-list)
    - [Limitations and future works](##limitations-and-future-work)
- [How to engage](#how-to-engage)
    - [As developers](#as-developers)
    - [As wallet users](#as-wallet-user)
    - [As fullnode maintainers](#as-fullnode-maintainer)
- [Feedback](#feedback)

## Description

Testnet is described in the [roadmap section](https://github.com/yeeco/yeeroot#roadmap) of yeeroot project description:

> Testnet (2020-03)

Testnet is an integrated release of all the former PoC (PoC-1 to PoC-7) releases. 

Testnet provides a relative stable protocol for building apps and other related software.

### Feature list
1. Full node

   * Full sharding
   * PoW block appending consensus
   * CRFG block finality consensus
   * Multi mining
   * Cross shard transaction
   * Sharding scalability
   
1. Switch

   * RPC proxy taking all the full nodes as the backends.
   * Mining RPC for external miners.  
     [Rpc description](https://github.com/yeeco/wiki/wiki/Switch-RPC-Description#mining_submitJob)
   
1. Block chain explorer
   
   * with which users can browse and search for any blocks, transactions, accounts or events.  
   [URL](https://testnet.yeescan.org/)
   
1. Web wallet

   * with which users can create or import wallet, check balance and make transfers.  
   [URL](https://pocnet.yeescan.org/)  
   Click the "Wallet" at the top right.


### Limitations and future works
 
1. Now the PoW hashing algorithm is Blake2b256, which may be modified.

## How to engage

### As Developers
If you are block chain app developers, you can check if there are projects interesting you 
in our [Bounty Program](https://github.com/yeeco/wiki/wiki/Bounty-Program).

### As wallet users
If you just want to try with coin transfer, you can visit [TESTNET Explorer](https://testnet.yeescan.org),
where you can create or import wallet, check balance and make transfers.

### As full node maintainers
If you want to run a full node and play it by RPC, you can follow the steps.

1. Download [official docker image](https://hub.docker.com/r/yeeco/yeeroot) yeeco/yeeroot:20200330
1. Start docker container, with a picked shard number
    ```
    docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666
    ``` 

1. If you'd like to join in POW mining.
    
    1. Way 1: single mining
    
        ```
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere --mine
        ```
             
    1. Way 2: multi mining
    
        1 ) Start 4 nodes of the 4 shards
        ```
        mkdir -p ./data/yee
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee --shard-num=0 --rpc_port=9033 --ws_port=9044 --port=30333 --foreign_port=30334 --bath-path=/data/yee/shard_0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee --shard-num=1 --rpc_port=9133 --ws_port=9144 --port=31333 --foreign_port=31334 --bath-path=/data/yee/shard_1 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee --shard-num=2 --rpc_port=9233 --ws_port=9244 --port=32333 --foreign_port=32334 --bath-path=/data/yee/shard_2 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee --shard-num=3 --rpc_port=9333 --ws_port=9344 --port=33333 --foreign_port=33334 --bath-path=/data/yee/shard_3 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        ```
   
        2 ) Prepare switch (works as a multi-miner) configuration file
        
        file path: ./data/yee/conf/switch.toml
        
        file content: 
        ```
        [shards]
        [shards.0]
        rpc = ["http://127.0.0.1:9033"]
        
        [shards.1]
        rpc = ["http://127.0.0.1:9133"]
        
        [shards.2]
        rpc = ["http://127.0.0.1:9233"]
        
        [shards.3]
        rpc = ["http://127.0.0.1:9333"]
        ```
        
        3 ) Start switch
        ```
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200330 yee switch --bath-path=/data/yee --mine
        ```

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
