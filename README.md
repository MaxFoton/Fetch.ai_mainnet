# Fetch.ai_mainnet
![ALT-logo](https://raw.githubusercontent.com/MaxFoton/Fetch.ai_mainnet/main/Fetch.png)

`Validator` Max Foton nodes

`Valoper` fetchvaloper1reqz9q7j5h7x39mmpqep720mxuxgrg5nxzmqma

`Delegate` https://www.mintscan.io/fetchai/validators/fetchvaloper1reqz9q7j5h7x39mmpqep720mxuxgrg5nxzmqma

`RPC` http://95.165.89.222:26655/

`Peer` 18c79856ef586d2e2dc910ac8b08ad0012d19c04@95.165.89.222:26656

**Start from statesync**

```
systemctl stop fetchd && fetchd tendermint unsafe-reset-all --home $HOME/.fetchd --keep-addr-book

curl https://raw.githubusercontent.com/MaxFoton/Fetch.ai_mainnet/main/addrbook.json > ~/.fetchd/config/addrbook.json
```

```
SNAP_RPC=95.165.89.222:26655 && \
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash) && \
echo $LATEST_HEIGHT $BLOCK_HEIGHT $TRUST_HASH
```

```
sed -i.bak -e  "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.fetchd/config/config.toml
sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.fetchd/config/config.toml
```

**Enable service and start**

```
sudo systemctl daemon-reload && sudo systemctl enable fetchd

sudo systemctl restart fetchd && sudo journalctl -fu fetchd -o cat
```

