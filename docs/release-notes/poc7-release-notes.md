
# YeeCo PoC-7 Release Notes

2019-02-14

## Table of Contents

- [Description](#description)
    - [Feture list](##feature-list)
    - [Limitations and future works](##limitations-and-future-work)
- [How to engage](#how-to-engage)
    - [As wallet users](#as-wallet-user)
    - [As fullnode maintainers](#as-fullnode-maintainer)
- [Feedback](#feedback)

## Description

PoC-7 is described in the [roadmap section](https://github.com/yeeco/yeeroot#roadmap) of yeeroot project description:

> PoC-7: Smart contract (on branch chain) (2020-02-14)

### Feature list
1. Token (like ERC-20)

   * Issue token.
   * Token transfer.
    
1. Web wallet

    A web wallet for POCNET is provided, 
    with which users can create or import wallet, check balance and make transfers.
      
    [https://pocnet.yeescan.org/create-wallet](https://pocnet.yeescan.org/create-wallet)         
    
1. Smart contract

    Smart contract is provided on the [yeebranch](https://github.com/yeeco/yeebranch).
     
    The [yeebranch](https://github.com/yeeco/yeebranch) is designed as layer 2 of YeeCo.
        
    The relationship of yeeroot and yeebranch is described as follows: 
    
    | Layer   | Name            |  Consensus   |  Transaction types   | 
    | --------| --------------- | ------------ |--------------| 
    | 1       | [Yeeroot chain](https://github.com/yeeco/yeeroot)   |  <ul><li>POW for block generation </li><li>CRFG for block finalization</li></ul> |  <ul><li> Native coin tx </li><li> Token tx </li><li> Meta tx of branch chains </li></ul>  |
    | 2       | [Yeebranch chain](https://github.com/yeeco/yeebranch) |  Optional <br> <ul><li>DPOS</li><li>POA</li></ul> | <ul><li> Basic tx </li><li> Smart contract tx </li>
    
    
### Limitations and future works
 
1. Token management and transfer are not supported yet on the web wallet.

## How to engage

### As wallet users
If you just want to try with coin transfer, you can visit [POCNET Explorer](https://pocnet.yeescan.org),
where you can create or import wallet, check balance and make transfers.

### As full node maintainers
If you want to run a full node and play it by RPC, you can follow the steps.

1. Download [official docker image](https://hub.docker.com/r/yeeco/yeeroot) yeeco/yeeroot:20200214
1. Start docker container, with a picked shard number
    ```
    docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666
    ``` 

1. If you'd like to join in POW mining.
    
    1. Way 1: single mining
    
        ```
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee --shard-num=0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere --mine
        ```
             
    1. Way 2: multi mining
    
        1 ) Start 4 nodes of the 4 shards
        ```
        mkdir -p ./data/yee
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee --shard-num=0 --rpc_port=9033 --ws_port=9044 --port=30333 --foreign_port=30334 --bath-path=/data/yee/shard_0 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee --shard-num=1 --rpc_port=9133 --ws_port=9144 --port=31333 --foreign_port=31334 --bath-path=/data/yee/shard_1 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee --shard-num=2 --rpc_port=9233 --ws_port=9244 --port=32333 --foreign_port=32334 --bath-path=/data/yee/shard_2 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee --shard-num=3 --rpc_port=9333 --ws_port=9344 --port=33333 --foreign_port=33334 --bath-path=/data/yee/shard_3 --bootnodes-routers=http://3.1.169.4:6666 --validator --coinbase=YourCoinBaseAddressHere
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
        docker run -d --network=host -v ./data:/data yeeco/yeeroot:20200214 yee switch --bath-path=/data/yee --mine
        ```

## Feedback
Feel free to dive in! [Open an issue](https://github.com/yeeco/yeeroot/issues/new).
