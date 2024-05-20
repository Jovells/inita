# Initia Node Setup Guide

## Installing the Node

To install the node, replace the Nodes Guru installation script with the following command:

```sh
wget -q -O initia.sh https://raw.githubusercontent.com/Jovells/inita/main/initia.sh && sudo chmod +x initia.sh && ./initia.sh && source $HOME/.bash_profile
```

This should get your snapshot downloaded and synced faster. After completing the installation, continue with the steps in the [Nodes Guru setup guide](https://nodes.guru/testnets/initia/setup-guide/en).

To check your node sync status, run:
local_height=$(initiad status | jq -r .sync_info.latest_block_height); network_height=$(curl -s https://rpc-initia-testnet.trusted-point.com/status | jq -r .result.sync_info.latest_block_height); blocks_left=$((network_height - local_height)); echo "Your node height: $local_height"; echo "Network height: $network_height"; echo "Blocks left: $blocks_left"

## Upgrade the node
Run:
```sh
cd $HOME/initia && git fetch --all && git checkout v0.2.15 && git pull origin v0.2.15 && make build && sudo mv $HOME/initia/build/initiad $(which initiad) && sudo systemctl restart initiad && sudo journalctl -u initiad -f
```

## How to Transfer a Node

### 1. Stop the Current Node

Disable the current node by running the following command:

```sh
sudo systemctl disable initiad
```

### 2. Copy the Validator State

To copy the validator state, run:

```sh
cat $HOME/.initia/data/priv_validator_state.json
```

Copy the output and keep it safe. You'll need it later.

### 3. Install Node on New Server

Follow the installation instructions provided above. Make sure to restore your old wallet during the wallet setup step instead of creating a new wallet.

### 4. Restore Validator State on New Node

To restore the validator state on the new node:

1. Open the `priv_validator_state.json` file:

   ```sh
   nano $HOME/.initia/data/priv_validator_state.json
   ```

2. Delete the current file content.
3. Paste the old validator state that you copied in step 2 using `Ctrl + U`.
4. Exit with `Ctrl + X`



---

By following these steps, you can successfully set up and transfer your Initia Node.
