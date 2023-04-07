
### web34ever
```
web3validator provides much more than security! We are actively participating in the development of the Network and Community by providing informational, technical and humanitarian support!
```
Telegram Uptime checker more [here](https://github.com/web3validator/SomethingAboutMyself/blob/main/web34ever-self-identity.md#uptime-checker-bot)


# <img width="399" alt="image" src="https://user-images.githubusercontent.com/59205554/230627135-067c1d73-0235-4732-8567-8708c099a520.png">

<img width="394" alt="image" src="https://user-images.githubusercontent.com/59205554/230626767-8fee34e3-ac7d-4b67-9eef-0f2b86e201c4.png">
#
https://t.me/AuraUpTime_bot

### Mainnet
* [Public API](https://github.com/web3validator/Guide_Web_Aura#public-api)
* [Install node](https://github.com/web3validator/Guide_Web_Aura#install-binary)
* [State sync](https://github.com/web3validator/Guide_Web_Aura#state-sync)



## Public API
<details>
  <summary><b>API</b></summary>

```
# RPC Endpoint
http://aura.web3validator.info:27557
  
# LCD (Rest) API Endpoint
http://aura.web3validator.info:1417
  
# GRPC Endpoint
http://aura.web3validator.info:9750
```
</details>

## Install binary

You can check new version here ([New_version_bin](https://github.com/aura-nw/aura/releases))
```
cd $HOME
git clone https://github.com/aura-nw/aura.git -b aura_v0.4.4
cd $HOME/aura
make install

```
```
aurad init web34ever --chain-id xstaxy-1
```

### Genesis
```
wget http://aura2.web3validator.info/genesis_aura.json -O $HOME/.aura/config/genesis.json

```
### Adderbook
```
wget http://aura2.web3validator.info/addrbook_aura.json -O $HOME/.aura/config/addrbook.json

```
### You need to install `aurad.service`
```
wget http://aura2.web3validator.info/aurad.service -P /etc/systemd/system/

```
If you want to quickly catch up with the network, use this [State Sync](https://github.com/web3validator/Guide_Web_Aura#state-sync)

```
sudo systemctl start aurad && sudo journalctl -u aurad -f --no-hostname -o cat
```



## State sync

<details >
  <summary><b>Pruning</b></summary>
  
  ```
  pruning = "default"
  
  pruning-keep-recent = "0"
  pruning-keep-every = "0"
  pruning-interval = "0"
  ```
  
</details>

  ```
  SNAP_RPC="http://aura.web3validator.info:27557"
  peers="7c032300fb320d452624f9a2e8c485a41fb427e9@aura.web3validator.info:27556"
  ```
  
  ### Let's put the height and trusthash to the-> `config.toml`
  ```
  LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
  BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
  TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) && \
  echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
  sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
  s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
  s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
  s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
  s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.aura/config/config.toml
  sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.aura/config/config.toml

  ```
  ### Reset the database 
  ```
  sudo systemctl stop aurad && 
  aurad tendermint unsafe-reset-all --home $HOME/.aura --keep-addr-book
  
  ```
  ## Restart node and check the logs
  ```
  sudo systemctl start aurad && sudo journalctl -u aurad -f --no-hostname -o cat
  ```

                                                           Contacts	


[<img src='https://user-images.githubusercontent.com/83868103/227937841-6e05b933-9534-49f1-808a-efe087a4f7cd.png' alt='Twitter'  width='16.5%'>](https://twitter.com/web34ever)[<img src='https://user-images.githubusercontent.com/83868103/227935592-ea64badd-ceb4-4945-8dfc-f25c787fb29d.png' alt='TELEGRAM'  width='16.5%'>](https://t.me/web34ever)[<img src='https://user-images.githubusercontent.com/59205554/229010780-8381c300-1eda-45c4-9d47-17d3abd32c98.png' alt='WEBSITE'  width='16.5%'>](https://web3validator.info)[<img src='https://user-images.githubusercontent.com/83868103/227936479-a48e814b-3ec1-4dcb-bd44-96b02d8f55da.png' alt='MAIL'  width='16.5%'>](mailto:web34ever@gmail.com)[<img src='https://user-images.githubusercontent.com/83868103/227948915-65731f97-c406-4d2c-996c-e5440ff67584.png' alt='GITHUB'  width='16.5%'>](https://github.com/web3validator)[<img src='https://user-images.githubusercontent.com/59205554/229010265-a3782434-0f41-4736-89c0-e50143da2abe.png' alt='Keybase'  width='16.5%'>](https://keybase.io/web34ever)






