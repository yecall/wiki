# Node deployment and mining

Project opening [Source](https://github.com/yeeco/yeeroot)

## Address generation method (Offline available)

Tool：yee-util

Download：[Click](https://github.com/yeeco/yee-utils/releases)

Instructions：[Click](https://github.com/yeeco/yee-utils/blob/master/docs/Usage.md#account-tools)

## Compile based on source code

Please refer to the [documentation](https://github.com/yeeco/yeeroot/blob/master/README.md#install)

## Deploy based on executable files

- [Download executable file](https://github.com/yeeco/yeeroot/releases)
  - Remarks: Currently, only executable files for Linux systems are provided. Suggested system: CentOS 7 x86_64
  
- Copy the file to the Linux system, the location is arbitrary, it is recommended to put under `/usr/local/bin`

- Example of single shard start command:
```shell script
yee --shard-num=0  --validator --coinbase=yee1yraer5mp29w607ultazarlhtfl08serxjxxktsfmkvcgl05mg5gqa47pax --base-path= /tmp/yee/shard_0 --log="info"
```

If you do not participate in mining, you can simply use:
```shell script
yee --shard-num=2 --base-path=/tmp/yee/shard_2
```

- Switch start command：
```shell script
yee switch --base-path=/tmp/yee/switch --rpc-port=10033 --rpc-external --enable-work-manager
```

- bootnodes-router start command：
```shell script
yee bootnodes-router --port=6666 --base-path=/tmp/yee/router
```

- Introduction to common startup parameters

 Parameter name | Instructions
 --- | ---
--shard-num | Node fragment number, after the first setting, it cannot be modified later and is a required option
--base-path  |Node data storage directory, it is strongly recommended to add this parameter
--log | Log output level settings, options: trace, debug, info, warn, error
--port | P2P port of the same shard
--foreign-port | P2P communication ports of different shards
--rpc-port| http service ports for the switch program to call
--rpc-external | By default (without this parameter) the rpc port can only be accessed locally, that is, the listening address is: 127.0.0.1:9933
--coinbase | Mining income address, if you don’t participate in mining, you don’t need to set this parameter
--validator | Voting option, you can only vote after mining is successful. If you do not participate in mining, you do not need to set this parameter
--enable-work-manager | Only valid in subcommand switch, if you need GPU mining, you need to configure this parameter

#### switch Configuration

Create a new conf directory under the base-path directory in the switch startup parameters, and create a file switch.toml in this directory. In this file, you need to configure the rpc information of 4 shards, the content format is as follows:
```toml
[shards]
[shards.0]
rpc = ["http://127.0.0.1:9930"]

[shards.1]
rpc = ["http://127.0.0.1:9931"]

[shards.2]
rpc = ["http://127.0.0.1:9932"]

[shards.3]
rpc = ["http://127.0.0.1:9933"]
```

#### bootnodes-router Configuration

Create a new conf directory under the base-path directory in the bootnodes-router startup parameters, and create a new file bootnodes-router.toml in this directory. In this file, you need to configure the P2P connection information of each segment, the content format is as follows:
```toml
[shards]
[shards.0]
native = ["/ip4/127.0.0.1/tcp/30330/p2p/QmSoFFFPH6GFQU1BmsCVQ7oH5R9N2p8MJhHU8sBKnWua9x"]
foreign = ["/ip4/127.0.0.1/tcp/31330/p2p/QmWJemf4ycUA2FWm4xjTYJERjzWNGYdTYzV8HLsHFLPsVj"]

[shards.1]
native = ["/ip4/127.0.0.1/tcp/30331/p2p/QmRaWWeKBUaB1cUKGQE6VXKYRCdZqrJnistPKuARLRa4NY"]
foreign = ["/ip4/127.0.0.1/tcp/31331/p2p/QmUXSVwZ231tazECxhz2ZJTMHWUdKN5sXvPa5oK29oy7TK"]

[shards.2]
native = ["/ip4/127.0.0.1/tcp/30332/p2p/QmUPvdiVr1nKt4d6EXZSLMdEshs7a5xPVP9zj4uNmszfbM"]
foreign = ["/ip4/127.0.0.1/tcp/31332/p2p/QmQLhj8PSzeZrCih3caCY7z4r2UyWjgtiPwUYsxVtoJ6ZQ"]

[shards.3]
native = ["/ip4/127.0.0.1/tcp/30333/p2p/QmT5JYjJHWPQ3Zwf5u9G6FJ3USLT6UBH5w6cfCsxwC9Nvt"]
foreign = ["/ip4/127.0.0.1/tcp/31333/p2p/QmSghhMSyXv9oxepjndzdTTyyrrSUAJtRbQuVvWb6zTxML"]
```

Note: Nodekey of each node can be seen through the log at startup, or through the yee-util tool, please refer to the relevant documents.

## Deploy nodes based on docker 
1.	Preconditions: Install the docker server, have a certain understanding of docker.
2.The yeeroot mirror can be searched by command `docker search yeeroot` under the name: `yeeco/yeeroot`, tag is the date of issue.
3. Draw a mirror of the recent date by `docker pull ***` command.
4.The total number of initial shards is 4, so 4 sharding node programs and one switch program need to be run, that is, 5 docker containers need to be run.
5.It is recommended to use docker-compose to start the docker program.
6.The startup method is to execute the `docker-compose up -d` command in the `docker-compose.yml` directory. For other commands, please check the official docker document.

#### docker-compose.yml File format (participation in mining)

```yaml
version: '2'
services:
    switch:
        restart: on-failure
        image: yeeco/yeeroot
        volumes:
            - ../../service/yeeroot/data_switch:/data
        network_mode: host
        ports:
            - '10033:10033'
        entrypoint:
            - yee
            - switch
            - --rpc-port=10033
            - --rpc-external
            - --enable-work-manager
    node-0:
        restart: always
        image: yeeco/yeeroot
        network_mode: host
        volumes:
            - ../../service/yeeroot/data_0:/data
        ports:
            - '30330:30330'
            - '9930:9930'
            - '31330:31330'
        entrypoint:
            - yee
            - --validator
            - --shard-num=0
            - --coinbase=${COINBASE_0}
            - --port=30330
            - --rpc-port=9930
            - --foreign-port=31330
            - --rpc-external
    node-1:
        restart: always
        image: yeeco/yeeroot
        network_mode: host
        volumes:
            - ../../service/yeeroot/data_1:/data
        ports:
            - '30331:30331'
            - '9931:9931'
            - '31331:31331'
        entrypoint:
            - yee
            - --validator
            - --shard-num=1
            - --coinbase=${COINBASE_1}
            - --port=30331
            - --rpc-port=9931
            - --foreign-port=31331
            - --rpc-external
    node-2:
        restart: always
        image: yeeco/yeeroot
        network_mode: host
        volumes:
            - ../../service/yeeroot/data_2:/data
        ports:
            - '30332:30332'
            - '9932:9932'
            - '31332:31332'
        entrypoint:
            - yee
            - --validator
            - --shard-num=2
            - --coinbase=${COINBASE_2}
            - --port=30332
            - --rpc-port=9932
            - --foreign-port=31332
            - --rpc-external
    node-3:
        restart: always
        image: yeeco/yeeroot
        network_mode: host
        volumes:
            - ../../service/yeeroot/data_3:/data
        ports:
            - '30333:30333'
            - '9933:9933'
            - '31333:31333'
        entrypoint:
            - yee
            - --validator
            - --shard-num=3
            - --coinbase=${COINBASE_3}
            - --port=30333
            - --rpc-port=9933
            - --foreign-port=31333
            - --rpc-external
```

- The `COINBASE_*` is placed in another environment variable configuration profile
- If you do not participate in mining, you can remove `--validator` and `--coinbase`
- Configure the switch.toml files required by the subcommand switch, please refer to the previous section for details

## Participation in mining

### Mining by CPU

Add the parameter `--mine` in the node startup parameters, and the CPU mining does not rely on the switch program.

### Mining by GPU

1. Start all the sharding nodes, the startup parameters need to have parameters `--validator --coinbase=***`
2. Start the switch program, please refer to the above part of the document for details.
3. Download the [miner](https://github.com/yeeco/ccminer-yee/releases).
4. Modify the configuration file to the parameters in the switch, the following configuration example is as follows: 
```
"algo" : "yee"
"url": "127.0.0.1:10033"
```
5. Only Nvidia graphics cards under windows platform are currently supported
