


![image](https://github.com/molla202/Cardchain-Testnet-6/assets/91562185/42753ec7-d7e2-4ae2-9f91-b58d3e50491c)












 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [CrowdControl Website](https://crowdcontrol.network/)<br>
 * [Blockchain Explorer](https://testnet.itrocket.net/cardchain/staking)<br>
 * [Discord](https://discord.gg/yYhRhK88SP)<br>
 * [CrowdControl Twitter](https://twitter.com/CrowdControlNet)<br>

## 💻 Sistem Gereksinimleri
| Bileşenler | Minimum Gereksinimler | 
| ------------ | ------------ |
| CPU |	4|
| RAM	| 8+ GB |
| Storage	| 400 GB SSD |


### 🚧Update ve gereklilikler
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```



### 🚧 Go 
```
cd $HOME
! [ -x "$(command -v go)" ] && {
VER="1.19.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
}
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```
### 🚧 Varyasyonları ayarlıyalım
```
Not: Aşağıdaki wallet ve moniker kısmına kendi adınızı giriniz...
echo "export WALLET="molla202"" >> $HOME/.bash_profile
echo "export MONIKER="molla202"" >> $HOME/.bash_profile
echo "export CARDCHAIN_CHAIN_ID="cardtestnet-6"" >> $HOME/.bash_profile
echo "export CARDCHAIN_PORT="31"" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
### 🚧 Dosyaları çekip kuruyoruz
```
cd $HOME

rm -rf Cardchain

git clone https://github.com/DecentralCardGame/Cardchain.git

cd Cardchain

git checkout v0.11.0

make install
```
### 🚧 Configurasyonlar
```
Cardchaind config node tcp://localhost:${CARDCHAIN_PORT}657
Cardchaind config keyring-backend os
Cardchaind config chain-id cardtestnet-6
Cardchaind init "$MONIKER" --chain-id cardtestnet-6
```
### 🚧 Genesis ve adressbook
```
wget -O $HOME/.Cardchain/config/genesis.json https://testnet-files.itrocket.net/cardchain/genesis.json
wget -O $HOME/.Cardchain/config/addrbook.json https://testnet-files.itrocket.net/cardchain/addrbook.json
```
### 🚧 Seed ve Peer
```
SEEDS="947aa14a9e6722df948d46b9e3ff24dd72920257@cardchain-testnet-seed.itrocket.net:31656"
PEERS="99dcfbba34316285fceea8feb0b644c4dc67c53b@cardchain-testnet-peer.itrocket.net:31656,d0e4edcdd73a7578b10980b3739a5b7218b7e86f@212.23.222.109:26256,1f0a4eac263a6c77ec7020dcdde1547af473df4f@185.249.227.91:26656,7dcbe1c7c24e849c0b89271ded17fb71dc61a7fe@95.216.35.51:21156,e39e90c93313f0c0bccde67e33c4341ac0c724fe@178.18.251.146:14656,5caecb793facd1605f3973397367bf61e9bebdc9@135.181.220.61:11656,78a2c6a4f6aeab1f24681ee0864f4546b615b48e@194.163.179.176:12356,542d3d320d50d7e4d4fe8abb8950f346f10fb106@142.132.202.86:16001,06cffa9c37b0d290ecef5ad2e48b3f0338da0b43@18.118.162.219:26656,2fd09544afaaf5ea07ccea00f2e437d1cc283f51@185.202.236.103:26656,2433afb5d241e24a68f81b66be5f4db3c83dfec3@88.198.47.154:46656"
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.Cardchain/config/config.toml
```
### 🚧 port
```
sed -i.bak -e "s%:1317%:${CARDCHAIN_PORT}317%g;
s%:8080%:${CARDCHAIN_PORT}080%g;
s%:9090%:${CARDCHAIN_PORT}090%g;
s%:9091%:${CARDCHAIN_PORT}091%g;
s%:8545%:${CARDCHAIN_PORT}545%g;
s%:8546%:${CARDCHAIN_PORT}546%g;
s%:6065%:${CARDCHAIN_PORT}065%g" $HOME/.Cardchain/config/app.toml
```
### 🚧 port
```
sed -i.bak -e "s%:26658%:${CARDCHAIN_PORT}658%g;
s%:26657%:${CARDCHAIN_PORT}657%g;
s%:6060%:${CARDCHAIN_PORT}060%g;
s%:26656%:${CARDCHAIN_PORT}656%g;
s%^external_address = \"\"%external_address = \"$(wget -qO- eth0.me):${CARDCHAIN_PORT}656\"%;
s%:26660%:${CARDCHAIN_PORT}660%g" $HOME/.Cardchain/config/config.toml
```
### 🚧 Puring
```
sed -i -e "s/^pruning *=.*/pruning = \"nothing\"/" $HOME/.Cardchain/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"100\"/" $HOME/.Cardchain/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"50\"/" $HOME/.Cardchain/config/app.toml
```
### 🚧 Gas ve diğer ayarlar
```
sed -i 's|minimum-gas-prices =.*|minimum-gas-prices = "0.0ubpf"|g' $HOME/.Cardchain/config/app.toml
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.Cardchain/config/config.toml
sed -i -e "s/^indexer *=.*/indexer = \"null\"/" $HOME/.Cardchain/config/config.toml
```
### 🚧 Servis Dosyası
```
sudo tee /etc/systemd/system/Cardchaind.service > /dev/null <<EOF
[Unit]
Description=Cardchain node
After=network-online.target
[Service]
User=$USER
WorkingDirectory=$HOME/.Cardchain
ExecStart=$(which Cardchaind) start --home $HOME/.Cardchain
Restart=on-failure
RestartSec=5
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF
```
### 🚧 Snap
```
Cardchaind tendermint unsafe-reset-all --home $HOME/.Cardchain
if curl -s --head curl https://testnet-files.itrocket.net/cardchain/snap_cardchain.tar.lz4 | head -n 1 | grep "200" > /dev/null; then
  curl https://testnet-files.itrocket.net/cardchain/snap_cardchain.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.Cardchain
    else
  echo no have snap
fi
```
### 🚧 Servisleri başlatalım
```
sudo systemctl daemon-reload
sudo systemctl enable Cardchaind
sudo systemctl restart Cardchaind && sudo journalctl -u Cardchaind -fo cat
```

### Cüzdan oluşturma yada import
```
Cardchaind keys add cüzdan-adı
```
OR import
```
Cardchaind keys add cüzdan-adı --recover
```

### Validator olusturma
```
Cardchaind tx staking create-validator \
--amount 1000000ubpf \
--from $WALLET \
--commission-rate 0.1 \
--commission-max-rate 0.2 \
--commission-max-change-rate 0.01 \
--min-self-delegation 1 \
--pubkey $(Cardchaind tendermint show-validator) \
--moniker "$MONIKER" \
--identity "" \
--details "" \
--chain-id cardtestnet-6 \
--gas auto --gas-adjustment 1.5 \
-y
```
