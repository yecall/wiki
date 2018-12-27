# Roadmap of YeeChain

## Milestones

* P2p network's main functions completed at the end of September,2018.
* On November 16 2018, YEE pubilic chain technical white paper and Tetris consensus algorithm paper were officially released.
* YEE mini blockchain demo exceeds 10k TPS (Transactions per second).

### v0.5 (2019 Q1)

* Test-net will be online.

### v1.0 (2019 Q3)

* Main-net will be released.

### v2.0 (2019 Q4)

*  support app engine.

## Version

### v0.1.0 [done]

Goals

* Minimal blockchain test with PoW;
* Core architecture completion;
* Simple P2P support.

### v0.2.0 [done]

Goals

* Command line and flags parse;
* Subcommand of console, account management etc;
* Node start/stop, integrated with p2p service, core, consensus, rpc etc.

Cmd

* Console with JS interpretor;
* Account management, create, list accounts.

Net

* P2p protocol design and implement;
* P2p bootstrap.

Crypto

* Implement secp256k1;
* Implement keystore, test with cipher of Scrypt, Argon2 and Balloon.

### v0.3.0 [done]

Goals

* Full function P2P network support.

Consensus

* Tetris poc test.

Net

* P2P bootnode and configuration;
* P2P node discover;
* P2P task scheduler;
* P2P dynamic configuration;
* P2P peer management.

Crypto

* Post quantum crypto test, with qTESLA.

Storage

* LevelDB persistent storage;
* Storage interface.

Misc

* Bundle of resource file, such as .toml config files, .js files etc.

### v0.4.0 [done]

Goals

* Tetris paper releas;
* A Tetris inside minimal demo blockchain;
* P2P DHT support.

Consensus

* Core algorithm of Tetris;
* Tetris demo, support a minimal blockchain to broadcast transactions and produce blocks, maintain account state.

Net

* Inmem P2P service;
* Implement DHT service, and support temporary DHT for consensus module;
* Implement sub-overlay p2p network;
* Support sub-overlay p2p network reconfiguration.




