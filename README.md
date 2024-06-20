# Avail-Turing
The Turing Testnet is live and marks Avail's final and definitive pre-Mainnet testing environment, aimed at vetting Mainnet deployments, executing extensive stress tests, and assessing network topologies for scalability and resilience. Validators are tasked with an important role in network orchestration, ensuring seamless integration and providing critical feedback for iterative improvements to the Avail team.

Website: https://www.availproject.org

Twitter: https://twitter.com/AvailProject

Discord: https://discord.gg/y6fHnxZQX8

Docs: https://docs.availproject.org/clash-of-nodes/challenges

Github: https://github.com/availproject/avail/releases

# Faucet
https://faucet.avail.tools/
# Explorer
https://explorer.avail.so/#/explorer
# Telemetry
https://telemetry.avail.so/#list/0xd3d2f3a3495dc597434a99d7d449ebad6616db45e4e4f178f31cc6fa14378b70
# Public RPC
```
https://avail-turing-rpc.nodesync.top
```
```
wss://avail-turing-rpc.nodesync.top
```
### use RPC
https://explorer.avail.so/?rpc=wss://avail-turing-rpc.nodesync.top
# Server preparation
System: `**Ubuntu 22.04**`
```
sudo apt update
sudo apt install wget curl make clang pkg-config libssl-dev build-essential jq lz4 gcc unzip snapd -y
```
# Download the binary file
```
sudo mkdir -p $HOME/avail-node && cd $HOME/avail-node
sudo wget https://github.com/availproject/avail/releases/download/v2.2.1.0-rc1/x86_64-ubuntu-2204-avail-node.tar.gz
sudo tar xvzf x86_64-ubuntu-2204-data-avail.tar.gz
sudo rm -rf x86_64-ubuntu-2204-avail-node.tar.gz
sudo chmod +x avail-node
cd $HOME
```
# Create a service 
NOTE: Change the section that says `NodeSync.Top`, your display name in explorer...
```
yournodename="NodeSync.Top"
```
```
sudo tee /etc/systemd/system/availd.service > /dev/null <<'EOF'
[Unit]
Description=Avail Turing Validator
After=network.target
StartLimitIntervalSec=0

[Service]
User=root
Type=simple
Restart=always
RestartSec=120
ExecStart=$HOME/avail-node/avail-node -d $HOME/avail-node/node-data --chain turing --validator --name $yournodename

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload
sudo systemctl enable availd
sudo systemctl restart availd 
```
# Check service & logs:
```
sudo systemctl status availd
```
```
sudo journalctl -u availd -f -o cat
```
# Session Keys
```
curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}' http://localhost:9944
```
# Snapshot
```
sudo systemctl stop availd
rm -rf $HOME/avail-node/node-data/chains/avail_turing_network/paritydb
wget https://files.nodesync.top/Avail-Turing/avail-turing-snap.tar.lz4 
lz4 -c -d avail-turing-snap.tar.lz4 | tar -x -C $HOME/avail-node/node-data/chains/avail_turing_network/
rm -rf avail-turing-snap.tar.lz4
```
```
sudo systemctl restart availd
sudo journalctl -u availd -f -o cat
```
# Upgrade
```
cd $HOME && sudo systemctl stop availd.service && cd $HOME/avail-node
rm -rf avail-node
wget https://github.com/availproject/avail/releases/download/v2.2.4.0-rc1/x86_64-ubuntu-2204-avail-node.tar.gz
tar -xf x86_64-ubuntu-2204-avail-node.tar.gz
rm -rf x86_64-ubuntu-2204-avail-node.tar.gz
cd $HOME
sudo systemctl daemon-reload
sudo systemctl enable availd
sudo systemctl restart availd && sudo journalctl -u availd -f -o cat
```


