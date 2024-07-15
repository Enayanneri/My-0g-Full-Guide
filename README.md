# My-0g-Full-Guide ![icons8-джейк](https://github.com/user-attachments/assets/e64c619e-a065-4cd9-b04a-309f838f3976)
This repository contains my complete guide to launching all nodes of the 0g_labs project, as well as a script that will allow you to do this faster!

# Testnet Conf
|key|value|
|:-:|:---:|
|Chain-Name|0G-Newton-Testnet|
|Chain-Id|16600 (zgtendermint_16600-2)|
|Token-Symbol|A0GI|
|Chain-RPC|https://rpc-testnet.0g.ai|
|Chain-Websocket|wss://rpc-testnet.0g.ai/ws|
|Storage-RPC|https://rpc-storage-testnet.0g.ai|
|Chain-Explorer|https://chainscan-newton.0g.ai/|
|Storage-Explorer|https://storagescan-newton.0g.ai/|

# Validator Node
## Hardware Req
|key|value|
|:-:|:---:|
|Memory|64 GB|
|CPU|8 cores|
|Disk|1 TB NVME SSD|
|Bandwidth|100 MBps|

## Install via CLI
```
git clone -b v0.2.3 https://github.com/0glabs/0g-chain.git
./0g-chain/networks/testnet/install.sh
source ~/.profile

0gchaind config chain-id zgtendermint_16600-2
```

## Init Node
```
0gchaind init <name> --chain-id zgtendermint_16600-2
```

## Genesis File Download
```
sudo apt install -y unzip wget
rm ~/.0gchain/config/genesis.json
wget -P ~/.0gchain/config https://github.com/0glabs/0g-chain/releases/download/v0.2.3/genesis.json
```

## Add seeds to config.toml
```
81987895a11f6689ada254c6b57932ab7ed909b6@54.241.167.190:26656,010fb4de28667725a4fef26cdc7f9452cc34b16d@54.176.175.48:26656,e9b4bc203197b62cc7e6a80a64742e752f4210d5@54.193.250.204:26656,68b9145889e7576b652ca68d985826abd46ad660@18.166.164.232:26656
# to
seeds = "<node-id>@<ip>:<p2p port>"
```

## Start Node
```
0gchaind start
```
