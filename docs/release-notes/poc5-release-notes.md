
# YeeCo PoC-5 Release Notes

2019-11-29

## Table of Contents

- [Description](#description)
    - [Feture list](##feature-list)
    - [Limitations and future works](##limitations-and-future-work)
- [How to engage](#how-to-engage)
    - [As wallet users](#as-wallet-user)
    - [As fullnode maintainers](#as-fullnode-maintainer)
- [Feedback](#feedback)

## Description

PoC-5 is described in the [roadmap section](https://github.com/yeeco/yeeroot#roadmap) of yeeroot project description:

>PoC-5: Dynamic sharding (2019-11)

### Feature list
1. Address format

    YeeCo address follows BIP39 standard, and use a coin type 4096:
    ```
    index   hexa        symbol  coin 
    4096    0x80001000  YEE     YeeCo
    ```
    [SLIP-44](https://github.com/satoshilabs/slips/blob/master/slip-0044.md)
    
    YeeCo address is encoded in the [bech32](https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki) format, and use a HRP:
    ```
    Coin    Mainnet     Testnet
    YeeCo   yee         tyee
    ```
    [SLIP-173](https://github.com/satoshilabs/slips/blob/master/slip-0173.md)
    
1. Block reward

    The miner of each new block is rewarded with YEE. The amount is calculated as follows:
    ```
    block_reward = 50 / shard_count + transaction_fees
    ```
    According to the `Conditional reward` design, 
    the reward will be delayed util the miner exercises all the voting rights granted by the new block mined.
                                                                                            
 
1. CRFG
    
    The yeeroot node can make consensus on block finalization by the voters which can be rotated when new block is mined.

1. Relay transaction verification

    The yeeroot node implements strict verification of relay transactions by taking advantage of MMTP (Multi merkle tree proof) technology.
 
1. Sharding scalability

    The yeeroot node implements smooth scaling out. 
    
1. Block interval improvement

    Block interval is improved to 30 seconds.
    
### Limitations and future works
 
1. To prevent a failure on finalization consensus, it's not recommended to change the authority key (used to exercise crfg voting). 
We will provide safe way to change authority key in further work.
    

## How to engage

### As wallet users
If you just want to try with token transfer, you can visit [poctnet page](https://pocnet.yeeco.io),
where you can create wallet, check balance and make transfers.

### As full node maintainers
If you want to run a full node and play it by RPC, you can follow the steps.

1. Download [official docker image](https://hub.docker.com/r/yeeco/yeeroot) yeeco/yeeroot:20191129
1. Start docker container, with a picked shard number
    ```
    docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666
    ``` 

1. If you'd like to join in POW mining.
    
    1. Way 1: single mining
    
        ```
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere --mine
        ```
             
    1. Way 2: multi mining
    
        1 ) Start 4 nodes of the 4 shards
        ```
        mkdir -p ./data/yee
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee --shard-num=0 --rpc_port=9033 --ws_port=9044 --port=30333 --foreign_port=30334 --bath-path=/data/yee/shard_0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee --shard-num=1 --rpc_port=9133 --ws_port=9144 --port=31333 --foreign_port=31334 --bath-path=/data/yee/shard_1 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee --shard-num=2 --rpc_port=9233 --ws_port=9244 --port=32333 --foreign_port=32334 --bath-path=/data/yee/shard_2 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee --shard-num=3 --rpc_port=9333 --ws_port=9344 --port=33333 --foreign_port=33334 --bath-path=/data/yee/shard_3 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
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
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20191129 yee switch --bath-path=/data/yee --mine
        ```

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
