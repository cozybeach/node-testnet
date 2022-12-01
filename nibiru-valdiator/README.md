# **NIBIRU CHAIN FULL NODE**

<p align="center">
  <img width="920" height="600" src="https://pbs.twimg.com/profile_banners/1516130689028087815/1667256106/1500x500">
</p>

## **HARDWARE SPECIFICATIONS**

| Component  | Recommendation |
| ------------- | ------------- |
| CPU | 2 CPU |
| MEMORY | 4gb RAM |
| STORAGE | 100gb SSD Storage |
| NETWORK | 100 Mbps of upload and download bandwidth |
| TYPE | VPS |
| OS | Ubuntu 20.04.5 LTS |

# **HOW TO RUN NIBIRU CHAIN FULL NODE**

## **INSTALL THE NIBID BINARY**

Run one of the following commands to determine if the ```amd64``` or ```arm64``` binaries will be required

```
dpkg --print-architecture
```

or

```
uname -m
```

**Download the binary from** [**NibiruChain-Release**](https://github.com/NibiruChain/nibiru/releases)

Unpack the folder

```
tar -xvf nibiru_linux_amd64.tar.gz && mv nibirud nibid
```

Add ```nibid``` binary to your ```$PATH```

```
export PATH=<path-to-nibid>:$PATH
```

## **INSTALL COSMOVISOR**

Install Cosmovisor from source

```
git clone https://github.com/cosmos/cosmos-sdk
cd cosmos-sdk
git checkout cosmovisor/v1.2.0
make cosmovisor
cp cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
cd $HOME
```

Setup environment variables

```
export DAEMON_NAME=nibid
export DAEMON_HOME=$HOME/.nibid
source ~/.profile
```

Create required directories

```
mkdir -p $DAEMON_HOME/cosmovisor/genesis/bin
mkdir -p $DAEMON_HOME/cosmovisor/upgrades
```

Add the genesis version of the binary

```
cp ~/go/bin/nibid $DAEMON_HOME/cosmovisor/genesis/bin
```

Create the service for Cosmovisor

```
sudo tee /etc/systemd/system/cosmovisor-nibiru.service<<EOF
[Unit]
Description=Cosmovisor for Nibiru Node
Requires=network-online.target
After=network-online.target

[Service]
Type=exec
User=<your_user>
Group=<your_user_group>
ExecStart=/home/<your_user>/go/bin/cosmovisor run start --home /home/<your_user>/.nibid
Restart=on-failure
RestartSec=3
Environment="DAEMON_NAME=nibid"
Environment="DAEMON_HOME=/home/<your_user>/.nibid"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_LOG_BUFFER_SIZE=512"
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

Enable the service

```
sudo systemctl daemon-reload
sudo systemctl enable cosmovisor-nibiru
```

## **1. UPDATE THE SYSTEM**

```
sudo apt update
sudo apt upgrade --yes
```

## **2. VERIFY NIBID VERSION**

```
nibid version
```

## **3. INIT THE CHAIN**

Innit the chain

```
nibid init <moniker-name> --chain-id=nibiru-testnet-1 --home $HOME/.nibid
```

Ceate a local key pair

```
nibid keys add <key-name>
nibid keys show <key-name> -a
```

Download the genesis file from the Tendermint RPC endpoint

```
curl -s https://rpc.testnet-1.nibiru.fi/genesis | jq -r .result.genesis > genesis.json
```

Then copy the genesis file to the ```$HOME/.nibid/config folder```

```
cp genesis.json $HOME/.nibid/config/genesis.json
```

Save the following text in a file named ```persistent_peers.txt```

```
f8610c8c491e8d18e8b566c47ec13f34176f451c@35.185.124.166:26656
ed6bec50bf3db42d7caa3e7e57e118f50c944dca@34.23.132.200:26656
```

Navigate to the directory with the ```persistent_peers.txt``` file and run

```
export PEERS=$(cat persistent_peers.txt| tr '\n' '_' | sed 's/_/,/g;s/,$//;s/^/"/;s/$/"/') && sed -i "s/persistent_peers = \"\"/persistent_peers = ${PEERS}/g" $HOME/.nibid/config/config.toml
```

Set minimum gas prices

```
sed -i 's/minimum-gas-prices =.*/minimum-gas-prices = "0.025unibi"/g' $HOME/.nibid/config/app.toml
```

Update block time parameters

```
CONFIG_TOML="$HOME/.nibid/config/config.toml"
 sed -i 's/timeout_propose =.*/timeout_propose = "100ms"/g' $CONFIG_TOML
 sed -i 's/timeout_propose_delta =.*/timeout_propose_delta = "500ms"/g' $CONFIG_TOML
 sed -i 's/timeout_prevote =.*/timeout_prevote = "100ms"/g' $CONFIG_TOML
 sed -i 's/timeout_prevote_delta =.*/timeout_prevote_delta = "500ms"/g' $CONFIG_TOML
 sed -i 's/timeout_precommit =.*/timeout_precommit = "100ms"/g' $CONFIG_TOML
 sed -i 's/timeout_precommit_delta =.*/timeout_precommit_delta = "500ms"/g' $CONFIG_TOML
 sed -i 's/timeout_commit =.*/timeout_commit = "1s"/g' $CONFIG_TOML
 sed -i 's/skip_timeout_commit =.*/skip_timeout_commit = false/g' $CONFIG_TOML
```

Start your node without a daemon

```
nibid start
```

Start your node with systemd

```
sudo systemctl start nibiru
```

Start your node with cosmovisor

```
sudo systemctl start cosmovisor-nibiru
```

Request token from [Nibiru Faucet](https://faucet.testnet-1.nibiru.fi/)

```
FAUCET_URL="https://faucet.testnet-1.nibiru.fi/"
ADDR="<YOUR_WALLET_ADDRESS"
curl -X POST -d '{"address": "'"$ADDR"'", "coins": ["10000000unibi","100000000000unusd"]}' $FAUCET_URL
```

## **4. BECAME VALIDATOR**

Save the Chain ID to your nibid config

```
nibid config chain-id nibiru-testnet-1
```

Create-validator transaction

```
nibid tx staking create-validator \
--amount 10000000unibi \
--commission-max-change-rate "0.1" \
--commission-max-rate "0.20" \
--commission-rate "0.1" \
--min-self-delegation "1" \
--details "<put your validator description there>" \
--pubkey=$(nibid tendermint show-validator) \
--moniker <your_moniker> \
--chain-id nibiru-testnet-1 \
--gas-prices 0.025unibi \
--from <key-name>
```
