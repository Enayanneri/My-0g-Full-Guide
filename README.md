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

## Create Validator
```
0gchaind keys add <key_name> --eth

0gchaind tx staking create-validator \
  --amount=<staking_amount>ua0gi \
  --pubkey=$(0gchaind tendermint show-validator) \
  --moniker="<your_validator_name>" \
  --chain-id=zgtendermint_16600-2 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --from=<key_name> \
  --gas=auto \
  --gas-adjustment=1.4
```

# Storage Node
## Hardware Req
|key|value|
|:-:|:---:|
|Memory|16 GB|
|CPU|4 cores|
|Disk|500 GB / 1 TB NVME SSD|
|Bandwidth|500 MBps|

## Clone storage Node source code and Build
```
git clone https://github.com/0glabs/0g-storage-client.git

cd 0g-storage-client
go build
```
## Run commands
```
./0g-storage-client upload --url <blockchain_rpc_endpoint> --contract <log_contract_address> --key <private_key> --node <storage_node_rpc_endpoint> --file <file_path>

./0g-storage-client download --node <storage_node_rpc_endpoint> --root <file_root_hash> --file <output_file_path>

./0g-storage-client download --node <storage_node_rpc_endpoint> --root <file_root_hash> --file <output_file_path> --proof
```

# Storage KV
## Hardware Req
|key|value|
|:-:|:---:|
|Memory|16 GB|
|CPU|4 cores|
|Disk|500 GB / 1 TB NVME SSD|
|Bandwidth|500 MBps|

## Clone source code
```
git clone -b v1.1.0-testnet https://github.com/0glabs/0g-storage-kv.git
```

## Build
```
cd 0g-storage-kv
git submodule update --init

cargo build --release
```

## Copy the config_example.toml to config.toml and update the parameters
```
# rpc endpoint
rpc_listen_address
# ips of storage service, separated by ","
zgs_node_urls = "http://ip1:port1,http://ip2:port2,..."

# layer one blockchain rpc endpoint
blockchain_rpc_endpoint

# flow contract address
log_contract_address

# block number to start the sync, better to align with the config in storage service
log_sync_start_block_number

# storage nodes to download data from, separated by ","
# the provided nodes should cover full data in the storage network
zgs_node_urls
```

## Run the kv service
```
cd run

# consider using tmux in order to run in background
../target/release/zgs_kv --config config.toml
```
