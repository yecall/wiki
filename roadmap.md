# Roadmap of YeeChain

## Milestones

* 2018 Q2，initial the project
* 2018 Q3，P2p network's main functions completed
* 2018 Q4, Core of Consensus algorithm Tetris finished, Tps of the demo mini blockchain exceed 10k.

### v0.5 (2019 Q1)

* Testnet will be online.

### v1.0 (2019 Q3)

* Mainnet will be released.

### v2.0 (2019 Q4)

* App engine supported

## Version

### v0.1.0 [done]

Goals

* Minimal blockchain test with Pow
* Core architecture of the project
* Simple p2p support

### v0.2.0 [done]

Goals

* Command line and flags parse.
* Subcommand of console, account management etc.
* Node start/stop, integrated with p2p service, core, consensus, rpc etc.

Cmd

* Console with JS interpretor
* Account management, create, list accounts

Net

* P2p protocol design and implement
* P2p bootstrap

Crypto

* Implement secp256k1
* Implement keystore, test with cipher of Scrypt, Argon2 and Balloon

### v0.3.0 [done]

Goals

* Full function p2p network support

Consensus

* Tetris poc test

Net

* P2p bootnode and configuration
* P2p node discover
* P2p task scheduler
* P2p dynamic configuration
* P2p peer management

Crypto

* Post quantum crypto test, with qTESLA

Storage

* LevelDB persistent storage
* Storage interface

Misc

* Bundle of resource file, such as .toml config files, .js files etc.

### v0.4.0 [done]

Goals

* Tetris paper released
* A Tetris inside minimal demo blockchain
* P2p dht support

Consensus

* Core algorithm of Tetris
* Tetris demo, support a minimal blockchain to broadcast transactions and produce block, maintain account state.

Net

* Inmem p2p service
* Implement DHT service, and support temporary DHT for consensus module.
* Implement sub-overlay p2p network
* Support sub-overlay p2p network reconfiguration




