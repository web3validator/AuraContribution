# Guide_Web_Aura


### web3validator
```
web3validator provides much more than security! We are actively participating in the development of the Network and Community by providing informational, technical and humanitarian support!
```


### Mainnet
* [Public API](https://github.com/web3validator/Guide_Web_Aura#public-api)
* [Install node](https://github.com/web3validator/Guide_Web_Aura#install-binary)
* [State sync](https://github.com/web3validator/Guide_Web_Aura#state-sync)



## Public API
<details>
  <summary><b>API</b></summary>

```
# RPC Endpoint
http://95.217.207.236:27557
# LCD (Rest) API Endpoint
http://95.217.207.236:1417
# GRPC Endpoint
http://95.217.207.236:9750
```
</details>

## Install binary

You can check new version here ([New_version_bin](https://github.com/aura-nw/aura/releases))
```
cd $HOME
git https://github.com/aura-nw/aura.git -b aura_v0.4.4
cd $HOME/aura
make install

```
### Genesis
```
wget http://65.108.66.34/genesis_aura.json -O $HOME/.aura/config/genesis.json

```
### Adderbook
```
wget http://65.108.66.34/addrbook_aura.json -O $HOME/.aura/config/addrbook.json

```
### You need to install `junod.service`
```
wget http://65.108.66.34/aurad.service -P /etc/systemd/system/

```
If you want to quickly catch up with the network, use this [State Sync](https://github.com/web3validator/Guide_Web_Aura#state-sync)

```
sudo systemctl start junod && sudo journalctl -u junod -f --no-hostname -o cat
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
  SNAP_RPС="http://65.108.66.34:28257"
  peers="a81c0e466aeaf1e785665f6ecc68bd5ca3d95b0e@65.108.66.34:28256"
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
  sudo systemctl stop junod && 
  junod tendermint unsafe-reset-all --home $HOME/.aura --keep-addr-book
  
  ```
  ## Restart node and check the logs
  ```
  sudo systemctl start junod && sudo journalctl -u junod -f --no-hostname -o cat
  ```

#######################################################################################

## State Sync Testnet


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
  SNAP_RPС="http://65.108.66.34:20087"
  peers="a81c0e466aeaf1e785665f6ecc68bd5ca3d95b0e@65.108.66.34:20086"
  ```

  ### Let's put the height and trusthash to the -> `config.toml`
  ```
  LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
  BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
  TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) && \
  echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
  sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
  s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
  s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
  s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
  s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.juno/config/config.toml
  sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.juno/config/config.toml

  ```
  ### Reset the database 
  ```
  sudo systemctl stop junod && 
  junod tendermint unsafe-reset-all --home $HOME/.juno --keep-addr-book

  ```
  ## Restart node and check the logs
  ```
  sudo systemctl start junod && sudo journalctl -u junod -f --no-hostname -o cat
  ```







