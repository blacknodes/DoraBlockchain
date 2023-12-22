# Dora Vota Testnet

# Setting Up Dependencies
```
sudo apt update && sudo apt upgrade -y
apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```
# Installing Golang
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
```
wget -O $HOME/.dora/config/genesis.json "https://raw.githubusercontent.com/blacknodes/DoraBlockchain/main/genesis.json"
wget -O $HOME/.dora/config/addrbook.json "https://raw.githubusercontent.com/blacknodes/DoraBlockchain/main/addrbook.json"
```
# Creating a Service File
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
```
sudo systemctl daemon-reload
sudo systemctl enable dorad
sudo systemctl restart dorad
```
# Check Logs
```
sudo journalctl -u dorad -f -o cat
```
