# Running Ark-Bitcoin Channel Service Locally

This service requires an Ark Listener and Bitcoin JSON RPC service running locally.


## Set up Bitcoin Network configuration

Install bitcoind (instructions below are for Ubuntu 16.04):

```
sudo apt-add-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install bitcoind
```

Create testnet data folder and add configuration file:

```
mkdir /data/bitcoin-testnet/
touch /data/bitcoin-testnet/bitcoin.conf
```

Add the following configuration to `/data/bitcoin-testnet/bitcoin.conf`:

```
rpcuser=bitcoinrpc
rpcpassword=change_this_to_a_long_random_password
testnet=1
rpcallowip=0.0.0.0/0
```

Start bitcoind testnet:

```
bitcoind -testnet -server -datadir=/data/bitcoin-testnet -txindex=1
```

It may take several hours for the bitcoin testnet to fully sync.


## Run Local Ark Listener

Follow the instructions on the [Running Ark Listener Locally](running-ark-listener-locally.md) guide. 


## Create Service Config

Create Ark network config file in `/etc/aces-ark-bitcoin-channel-service/ark-network.yml`:

```
scheme: http
version: 1.0
netHash: 6e84d08bd299ed97c212c886c98a57e36545c8f5d645ca7eeae63a8bd62d8988
seedPeers:
  -
    hostname: 5.39.9.240
    port: 4001
  -
    hostname: 5.39.9.241
    port: 4001
  -
    hostname: 5.39.9.242
    port: 4001
  -
    hostname: 5.39.9.243
    port: 4001
  -
    hostname: 5.39.9.244
    port: 4001
  -
    hostname: 5.39.9.250
    port: 4001
  -
    hostname: 5.39.9.251
    port: 4001
  -
    hostname: 5.39.9.252
    port: 4001
  -
    hostname: 5.39.9.253
    port: 4001
  -
    hostname: 5.39.9.254
    port: 4001
  -
    hostname: 5.39.9.255
    port: 4001
  -
    hostname: 5.39.53.48
    port: 4001
  -
    hostname: 5.39.53.49
    port: 4001
  -
    hostname: 5.39.53.50
    port: 4001
  -
    hostname: 5.39.53.51
    port: 4001
  -
    hostname: 5.39.53.52
    port: 4001
  -
    hostname: 5.39.53.53
    port: 4001
  -
    hostname: 5.39.53.54
    port: 4001
  -
    hostname: 5.39.53.55
    port: 4001
  -
    hostname: 37.59.129.160
    port: 4001
  -
    hostname: 37.59.129.161
    port: 4001
  -
    hostname: 37.59.129.162
    port: 4001
  -
    hostname: 37.59.129.163
    port: 4001
  -
    hostname: 37.59.129.164
    port: 4001
  -
    hostname: 37.59.129.165
    port: 4001
  -
    hostname: 37.59.129.166
    port: 4001
  -
    hostname: 37.59.129.167
    port: 4001
  -
    hostname: 37.59.129.168
    port: 4001
  -
    hostname: 37.59.129.169
    port: 4001
  -
    hostname: 37.59.129.170
    port: 4001
  -
    hostname: 37.59.129.171
    port: 4001
  -
    hostname: 37.59.129.172
    port: 4001
  -
    hostname: 37.59.129.173
    port: 4001
  -
    hostname: 37.59.129.174
    port: 4001
  -
    hostname: 37.59.129.175
    port: 4001
  -
    hostname: 193.70.72.80
    port: 4001
  -
    hostname: 193.70.72.81
    port: 4001
  -
    hostname: 193.70.72.82
    port: 4001
  -
    hostname: 193.70.72.83
    port: 4001
  -
    hostname: 193.70.72.84
    port: 4001
  -
    hostname: 193.70.72.85
    port: 4001
  -
    hostname: 193.70.72.86
    port: 4001
  -
    hostname: 193.70.72.87
    port: 4001
  -
    hostname: 193.70.72.88
    port: 4001
  -
    hostname: 193.70.72.89
    port: 4001
  -
    hostname: 193.70.72.90
    port: 4001
    
trustedPeers:
  -
    hostname: 52.35.9.191
    port: 4001
```

Create Service config file in `/etc/aces-ark-bitcoin-channel-service/application.yml`:

```
serverInfo:
  name: "Aces ARK-BTC Channel Service"
  description: "ACES ARK to BTC Channel service for value transfers."
  instructions: >
    After this contract is executed, any ARK sent to depositArkAddress will be exchanged for BTC and 
    sent directly to the given recipientBtcAddress less service fees.
  version: "1.0.0"
  websiteUrl: "https://arkaces.com"
  flatFee: "0"
  percentFee: "1.00%"
  capacities:
    -
      value: "5000.00"
      unit: "BTC"
      displayValue: "5000 BTC"

arkNetworkConfigPath: "file:/etc/aces-ark-bitcoin-channel-service/ark-network.yml"

fees:
  btcFlatFee: 0
  btcPercentFee: 1

server:
  port: 9191

bitcoinRpc:
  url: http://localhost:18332/
  username: bitcoinrpc
  password: change_this_to_a_long_random_password
  
arkListener:
  url: http://localhost:9091

arkEventCallbackUrl: http://localhost:9191/arkEvents
```

## Running Service 

```
git clone https://github.com/ark-aces/aces-ark-bitcoin-channel-service
cd aces-ark-bitcoin-channel-service
mvn spring-boot:run -Dspring.config.location=file:/etc/aces-ark-bitcoin-channel-service/application.yml
```


## Testing Service

The service will be available at the following URL:

```
curl http://localhost:9191/
```


### Create Bitcoin Address

Create a bitcoin address locally to receive funds from the channel service with:

```
bitcoin-cli  -datadir=/data/bitcoin-testnet -testnet getnewaddress
```

Copy the returned bitcoin address to use as `recipientBtcAddress` below:

```
muWWAMMKpKLb7toJrHscHXF91f87ZVkuNW
```

### Create channel

```
curl -X POST http://localhost:9191/contracts \
-H 'Content-type: application/json' \
-d '{
  "arguments": {
    "recipientBtcAddress": "muWWAMMKpKLb7toJrHscHXF91f87ZVkuNW"
  }
}' 
```
```
{
    "id": "uLoDUXYsSdQUHD1lSA9E",
    "createdAt": "2018-01-24T12:06:42.639Z",
    "status": "executed",
    "results": {
        "recipientBtcAddress": "muWWAMMKpKLb7toJrHscHXF91f87ZVkuNW",
        "depositArkAddress": "AP6oYtGHLaFR95hJizWAF8tKnRiSdMDfbc",
        "transfers": []
    }
}
```

### Create Deposit Ark transaction

Send ark transaction to the `depositArkAddress` returned in the channel contract.


### List Channel Transfers

The transfers should be completed in less than a minute. 

```
curl -X GET http://localhost:9191/contracts/uLoDUXYsSdQUHD1lSA9E
```

```
{
    "id": "uLoDUXYsSdQUHD1lSA9E",
    "createdAt": "2018-01-24T12:06:42.639Z",
    "status": "executed",
    "results": {
        "recipientBtcAddress": "muWWAMMKpKLb7toJrHscHXF91f87ZVkuNW",
        "depositArkAddress": "AP6oYtGHLaFR95hJizWAF8tKnRiSdMDfbc",
        "transfers": [
            {
                "id": "YCRv5QIHdcxOrhBCPy1G",
                "status": "complete",
                "createdAt": "2018-01-24T12:45:08.125Z",
                "arkTransactionId": "533181a2673e9a1390fe8917ef4c23677a36502ae99c18e61fae6d740ed122ec",
                "arkAmount": "0.10000000",
                "arkToBtcRate": "0.00059180",
                "arkFlatFee": "0.00000000",
                "arkPercentFee": "1.00000000",
                "arkTotalFee": "0.00100000",
                "btcSendAmount": "0.00005859",
                "btcTransactionId": "5a2a4f37b46bb9d4910f5f19cf88805af89da7058ef2acfe5ab78da09b63ab24"
            }
        ]
    }
}
```

