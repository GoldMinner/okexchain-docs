# Join the Public Testnet 

See the [testnet repo](https://github.com/okx/testnets) for
information on the latest testnet, including the correct version
of OKC to use and details about the genesis file.

**You need to [install okc](/dev/quick-start/install-okc.html) before you go further**


Details of deployment information: https://github.com/okx/testnets/blob/master/README.md
## Supported Platforms

We support running a full node on `Mac OS X`, `Windows` and `Linux`.

## Minimum System Requirements

The hardware must meet certain requirements to run `exchaind`.

For node requirement details, please visit [Node Requirement](/dev/nodes/node-requirement/node-requirement.html)

## Setting Up a New Node

These instructions are for setting up a brand new full node from scratch.

First, initialize the node and create the necessary config files:

```bash
exchaind init <your_custom_moniker> --chain-id exchain-65
```

> _NOTE_:
Monikers can contain only ASCII characters. Using Unicode characters will render your node unreachable.


You can edit this `moniker` later, in the `~/.exchaind/config/config.toml` file:

```toml
# A custom human readable name for this node
moniker = "<your_custom_moniker>"
```

You can edit the `~/.exchaind/config/exchaind.toml` file in order to enable the anti spam mechanism and reject incoming transactions with less than the minimum gas prices:

```
# This is a TOML config file.
# For more information, see https://github.com/toml-lang/toml

##### main base config options #####

# The minimum gas prices a validator is willing to accept for processing a
# transaction. A transaction's fees must meet the minimum of any denomination
# specified in this config (Our recommended quantity is  10^-9 okt).

minimum-gas-prices = ""
```

Your full node has been initialized! 

## Genesis & Seeds

### Copy the Genesis File

Fetch the testnet's `genesis.json` file into `exchaind`'s config directory.

Note we use the `latest` directory in the [testnets repo](https://github.com/okx/testnets) which contains details for the testnet like the latest version and the genesis file. 

To verify the correctness of the configuration run:

```bash
exchaind start --chain-id exchain-65
```

### Add Seed Nodes

Your node needs to know how to find peers. You'll need to add healthy seed nodes to `$HOME/.exchaind/config/config.toml`. The [testnets repo](https://github.com/okx/testnets) contains links to some seed nodes.

You can add `seeds` in the `~/.exchaind/config/config.toml` file:

```toml
# Comma separated list of seed nodes to connect to
seeds = "b7c6bdfe0c3a6c1c68d6d6849f1b60f566e189dd@3.13.150.20:36656,d7eec05e6449945c8e0fd080d58977d671eae588@35.176.111.229:36656,223b5b41d1dba9057401def49b456630e1ab2599@18.162.106.25:36656"
```

For more information on seeds and peers, you can [read this](https://docs.tendermint.com/master/spec/p2p/peer.html).

## Starting a New Node

Start the full node with this command:

```bash
exchaind start --chain-id exchain-65
```

Check that everything is running smoothly:

```bash
exchaincli status
```

See the [testnet repo](https://github.com/okx/testnets) for information on testnets, including the correct version of the OKC to use and details about the genesis file.

## JSON-RPC Endpoint
[RPC URL](/dev/api/okc-api/grpc-api.html)

## Upgrading Your Node

These instructions are for full nodes that have ran on previous versions of and would like to upgrade to the latest testnet.

### Reset Data

First, remove the outdated files and reset the data.

```bash
rm $HOME/.exchaind/config/addrbook.json $HOME/.exchaind/config/genesis.json
exchaind unsafe-reset-all
```

Your node is now in a pristine state while keeping the original `priv_validator.json` and `config.toml`. If you had any sentry nodes or full nodes setup before,
your node will still try to connect to them, but may fail if they haven't also
been upgraded.

> _NOTE_:
Make sure that every node has a unique `priv_validator.json`. Do not copy the `priv_validator.json` from an old node to multiple new nodes. Running two nodes with the same `priv_validator.json` will cause you to double sign.


### Software Upgrade

Now it is time to upgrade the software:

```bash
git clone https://github.com/okx/exchain.git
cd exchain
git fetch --all && git checkout master
make install
```

> _NOTE_: If you have issues at this step, please check that you have the latest stable version of GO installed.

Note we use `master` here since it contains the latest stable release.
See the [testnet repo](https://github.com/okx/testnets) for details on which version is needed for which testnet, and the [OKC release page](https://github.com/okx/exchain/releases) for details on each release.

Your full node has been cleanly upgraded!

### Upgrade to Validator Node

You now have an active full node. What's the next step? You can upgrade your full node to become a OKC Validator. The top 100 validators have the ability to propose new blocks to the OKC. Continue onto [the Validator Setup](/dev/core-concepts/validator/validators-guide-cli.md).

# Join the Public Testnet With Docker
## Run OKC testnet fullnode with docker

### 1. Download the docker image

```
docker pull okexchain/fullnode-testnet:latest
```

### 2. Set the data directory


Download the testnet snapshot from [here](/dev/resources/okc-snapshot/snapshot.html), and unzip it into a data directory ${DATA_DIR}.



### 3. Run docker container
```
docker run -d --name exchain-testnet-fullnode -v ${DATA_DIR}:/root/.exchaind/data/ -p 8545:8545 okexchain/fullnode-testnet:latest
```
`Notice: ${DATA_DIR} has to be an absolute path`


### 4. View the running log
```
docker logs --tail 100 -f exchain-testnet-fullnode
```

### 5. Stop and restart the fullnode
- Stop
```
docker stop exchain-testnet-fullnode
```
- Restart
```
docker start exchain-testnet-fullnode
```

### 5. RPC
When docker gets to the latest block, local RPC can be used：`http://localhost:8545`
