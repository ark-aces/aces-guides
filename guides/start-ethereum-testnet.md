# Start Ethereum Testnet

Ethereum has multiple testnets available. This guide is for setting up the 
[ropstein testnet](https://ropsten.etherscan.io/) in Ubuntu 16.04.


## Install dependencies

```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update

sudo apt-get install software-properties-common build-essential
sudo apt-get install ethereum solc
```

## Start geth

```
geth --testnet \
--datadir "/media/ethereum-ropstein-testnet" \
--fast \
--bootnodes "enode://20c9ad97c081d63397d7b685a412227a40e23c8bdc6688c6f37e97cfbc22d2b4d1db1510d8f61e6a8866ad7f0e17c02b14182d37ea7c3c8b9c2683aeb6b733a1@52.169.14.227:30303,enode://6ce05930c72abc632c58e2e4324f7c7ea478cec0ed4fa2528982cf34483094e9cbc9216e7aa349691242576d552a2a56aaeae426c5303ded677ce455ba1acd9d@13.84.180.240:30303"
```

## Connect to geth

```
geth --testnet attach ipc:/media/ethereum-ropstein-testnet/geth.ipc
```


## Create an account in geth

```
var password = "change-me";
personal.newAccount(password);
```

## Mine eth in geth

```
personal.unlockAccount(eth.coinbase);

miner.start(2);
admin.sleepBlocks(1);
```

