## Bitcoin Setup for ACES Nodes

Install bitcoind (instructions below are for Ubuntu 16.04):

```
sudo apt-add-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install bitcoind
```

Create testnet data folder and add configuration file:

```
mkdir /data/bitcoin/
touch /data/bitcoin/bitcoin.conf
```

Add the following configuration to `/data/bitcoin/bitcoin.conf`:

```
rpcuser=bitcoinrpc
rpcpassword=change_this_to_a_long_random_password

# only allow connections from localhost
rpcallowip=127.0.0.1/0
```

Start bitcoind using `txindex` and pruning to reduce required hard disk space:

```
bitcoind -server -datadir=/data/bitcoin -txindex=1 -prune=550
```

It may take a long time for the bitcoin to fully sync.