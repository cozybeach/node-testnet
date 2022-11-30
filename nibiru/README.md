# **NIBIRU CHAIN FULL NODE**

<p align="center">
  <img width="460" height="300" src="https://pbs.twimg.com/profile_banners/1516130689028087815/1667256106/1500x500">
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

Add ```nibid``` bianry to your ```$PATH```

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

Copy the genesis file to the ```$HOME/.nibid/config```

```
curl -s https://rpc.testnet-1.nibiru.fi/genesis | jq -r .result.genesis > genesis.json
```

Then copy the genesis file to the ```$HOME/.nibid/config folder```

```
cp genesis.json $HOME/.nibid/config/genesis.json
```

