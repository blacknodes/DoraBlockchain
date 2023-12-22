# Dora Vota Testnet
# [ONECLICKINSTALLATION](https://github.com/blacknodes/OneClickNodes/blob/main/Dora/README.md)


# Step By Step Guide
# Setting Up Dependencies
First, update your system and install necessary dependencies:
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
# Installing Golang
Download and install Golang:
```
ver="1.20.5"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

# Installing The App
Clone the Dora Vota repository and install it:
```
cd $HOME
git clone https://github.com/DoraFactory/doravota.git
cd doravota
git checkout 0.2.0
make install
```
# Create Wallet 
```
dorad keys add <wallet>
```
Replace <wallet> with suitable wallet name as per your choice.
# Set Your Validator Moniker
```
dorad init <yourvalidatorname> --chain-id vota-vk
dorad config chain-id vota-vk
```
Replace <yourvalidatorname> with suitable name as per your choice
# Download genesis and addrbook file
Get the genesis and address book files for the network:
```
wget -O $HOME/.dora/config/genesis.json "https://raw.githubusercontent.com/blacknodes/DoraBlockchain/main/genesis.json"
wget -O $HOME/.dora/config/addrbook.json "https://raw.githubusercontent.com/blacknodes/DoraBlockchain/main/addrbook.json"
```
# Creating a Service File
Set up the Dora node as a system service:
```
sudo tee /etc/systemd/system/dorad.service > /dev/null <<EOF
[Unit]
Description=dorad
After=network-online.target

[Service]
User=$USER
ExecStart=$(which dorad) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
# Start Dora Node as Service
Enable and start the Dora service:
```
sudo systemctl daemon-reload
sudo systemctl enable dorad
sudo systemctl restart dorad
```
# Check Logs
Monitor the logs to ensure everything is running smoothly:
```
sudo journalctl -u dorad -f -o cat
```
# Create Validator
Finally, create your validator node on the network (replace <wallet_name> and <validator_name> with your specific details):
```
dorad tx staking create-validator \
--commission-rate 0.1 \
--commission-max-rate 1 \
--commission-max-change-rate 1 \
--min-self-delegation "1" \
--amount 1000000000000000000peaka \
--pubkey $(dorad tendermint show-validator) \
--from <wallet> \
--moniker="yourvalidatorname" \
--chain-id vota-vk \
--fees 30000000000000000peaka \
--identity="" \
--website="" \
--details="" -y
```
