# 节点部署与挖矿

[项目开源地址](https://github.com/yeeco/yeeroot)

## 地址生成方法（可离线）
使用工具：**yee-util**

下载地址：[点击](https://github.com/yeeco/yee-utils/releases)

使用方法：[点击](https://github.com/yeeco/yee-utils/blob/master/docs/Usage.md#account-tools)


## 基于源码进行编译
请参考[文档](https://github.com/yeeco/yeeroot/blob/master/README.md#install)

## 基于可执行文件部署
- 下载可执行文件 [下载地址](https://github.com/yeeco/yeeroot/releases)
  - 备注：目前仅提供Linux系统的可执行文件，建议系统：CentOS 7 x86_64
  
- 将文件拷贝到Linux系统中，位置随意，建议放在`/usr/local/bin`下

- 单个分片启动命令范例：
```shell script
yee --shard-num=0  --validator --coinbase=yee1yraer5mp29w607ultazarlhtfl08serxjxxktsfmkvcgl05mg5gqa47pax --base-path= /tmp/yee/shard_0 --log="info"
```
若不参与挖矿，可简单的使用：
```shell script
yee --shard-num=2 --base-path=/tmp/yee/shard_2
```

- `switch`启动命令
```shell script
yee switch --base-path=/tmp/yee/switch --rpc-port=10033 --rpc-external --enable-work-manager
``` 

- `bootnodes-router`启动命令
```shell script
yee bootnodes-router --port=6666 --base-path=/tmp/yee/router
```

- 常用启动参数介绍

参数名 | 使用说明
--- | ---
--shard-num | 节点分片号，首次设置后，后续不能修改，且为必选项
--base-path | 节点数据存储目录，强烈建议添加该参数
--log | 日志输出级别设置，可选项：`trace、debug、info、warn、error`
--port | 同分片P2P端口
--foreign-port | 不同分片P2P通信端口
--rpc-port | http服务端口，供switch程序调用
--rpc-external | 默认情况下（无该参数）rpc端口是只能本地访问，即监听地址为：`127.0.0.1:9933`
--coinbase | 挖矿收益地址，若不参与挖矿，无需设置该参数
--validator | 投票选项，近在挖矿成功后可投票，若不参与挖矿，无需设置该参数
--enable-work-manager | 近在子命令`switch`中有效，如需GPU挖矿则需配置该参数


#### switch配置

在switch启动参数里的base-path目录下新建`conf`目录，在该目录下新建文件`switch.toml`，在该文件中需要配置4个分片的rpc信息，内容格式如下：
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

#### bootnodes-router配置

在bootnodes-router启动参数里的base-path目录下新建`conf`目录，在该目录下新建文件`bootnodes-router.toml`，在该文件中需要配置各个分片的P2P连接信息，内容格式如下：
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

备注：各节点nodekey在启动时可通过日志看到，也可以通过yee-util工具查看，具体请参考yee-util相关文档


## 基于docker部署节点
1. 前置条件：安装docker的服务器，对docker有一定了解
2. 通过命令`docker search yeeroot`可以搜索yeeroot镜像，镜像名为：`yeeco/yeeroot`，tag为发行日期
3. 通过`docker pull ***`命令拉取最近日期的镜像
4. 起始分片总数为4，所以需要运行4个分片节点程序与一个switch程序，即需要运行5个docker容器。
5. 建议使用docker-compose来启动docker程序
6. 启动方式为在`docker-compose.yml`目录下执行`docker-compose up -d`命令，其他命令请查看docker官方文档

#### docker-compose.yml文件格式(参与挖矿)
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
- 其中的COINBASE放在另外的环境变量配置文件中
- 若不参与挖矿可去掉`--validator`与`--coinbase`
- 配置子命令`switch`所需的`switch.toml`文件，具体请参考上节内容

## 参与挖矿

### CPU挖矿
在节点启动参数中添加参数`--mine`即可，且CPU挖矿不依赖`switch`程序。

### GPU挖矿

1. 将所有分片节点启动起来，启动参数里需要有参数`--validator --coinbase=***`
2. 启动`swith`程序，具体请参考该文档上述部分
3. 下载[挖矿程序](https://github.com/yeeco/ccminer-yee/releases)
4. 修改配置文件为switch中的参数，配置范例如下：
`"algo" : "yee"`
`"url": "127.0.0.1:10033"`
5. 目前仅支持windows平台下的Nvidia显卡
