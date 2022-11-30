# **BITCANNA VALIDATOR & SENTRY NODE**

## **Hardware Specifications**

| Component  | Recommendation |
| ------------- | ------------- |
| CPU  | 4 CPU |
| Memory  | 8gb RAM |
| Storage  | 200gb disk space |
| Network | 100 Mbps of upload and download bandwidth |
| Type | VPS **VDS preferred** |

# **HOW TO RUN BITCANNA VALIDATOR & SENTRY NODE**

## **INSTALL SOFTWARE**

```
sudo apt-get install build-essential jq
```

## **INSTALL GOLANG 1.15.X**

Remove any old GoLang installation

```
sudo rm -rf /usr/local/go
```

Download the software

```
curl https://dl.google.com/go/go1.15.11.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf -
```

Update environment variables

```
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export GOBIN=$HOME/go/bin
export PATH=$PATH:/usr/local/go/bin:$GOBIN
EOF
source $HOME/.profile
```

To check if GoLang already installed

```
go version
```

## **COMPILE BCAND SOURCE CODE**

Download Source Code and Compile

```
git clone https://github.com/BitCannaGlobal/testnet-bcna-cosmos.git
cd testnet-bcna-cosmos
git checkout v0.testnet7
make build   #it build the binary in build/ folder
```

Check the version

```
build/bcnad version
```

> The outpus should be ```0.testnet7```

## **INSTALL COSMOSVISOR**

```
go get github.com/cosmos/cosmos-sdk/cosmovisor/cmd/cosmovisor
```

> The installation might need few minutes
