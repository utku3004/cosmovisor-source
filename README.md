# Cosmovisor Kurulumu:
```
git clone https://github.com/cosmos/cosmos-sdk
cd cosmos-sdk
git checkout v0.42.7
make cosmovisor
cp cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
cd $HOME
```
```
export DAEMON_NAME=sourced
export DAEMON_HOME=$HOME/.source
```
```
source ~/.profile
```
# Node ismini kontrol ediyoruz:
```
echo $DAEMON_NAME
```

# Cosmovisor klasörleri yapılandırıyoruz:
```
mkdir -p $DAEMON_HOME/cosmovisor/genesis/bin
mkdir -p $DAEMON_HOME/cosmovisor/upgrades
```
# Source dosyalarını cosmovisor altına taşıyoruz:
```
cp $HOME/go/bin/rebusd $DAEMON_HOME/cosmovisor/genesis/bin
```

# Servis dosyasını güncelliyoruz
```
sudo nano /etc/systemd/system/sourced.service
```
# Servis dosyasını düzenliyoruz
```
[Unit]
Description=cosmovisor
After=network-online.target

[Service]
User=root
ExecStart=/home/root/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=4096
Environment="DAEMON_NAME=sourced"
Environment="DAEMON_HOME=/home/<your-user>/.source"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_LOG_BUFFER_SIZE=512"

[Install]
WantedBy=multi-user.target
```
# Node muzu başlatıyoruz:
```
sudo -S systemctl daemon-reload
sudo -S systemctl enable cosmovisor
sudo systemctl start cosmovisor
```

# Cosmovisor kontrol
```
sudo systemctl status cosmovisor
```
# Log kontrol
```
journalctl -u cosmovisor -f
