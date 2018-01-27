# Running Bitcoin Listener Locally

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


## Set up ACES Bitcoin Listener configuration

Create listener app config in `/etc/aces-listener-bitcoin/application.yml`:

```
serverInfo:
  name: "AcesListenerBitcoin"
  description: "ACES Listener implementation for Bitcoin"
  version: "1.0.0"
  websiteUrl: "https://arkaces.com"

maxScanBlockDepth: 4

bitcoinRpc:
  url: http://localhost:18332/
  username: bitcoinrpc
  password: change_this_to_a_long_random_password

server:
  port: 9090

arkAuth:
  requireAuth: false

logging:
  level:
    com.arkaces.aces_listener_bitcoin.BitcoinEventListener: ERROR
    com.arkaces.aces_listener_bitcoin.bitcoin.BitcoinService: ERROR
```

### Local Datasource

Add the following configuration to `application.yml` to enable data persistence
between application restarts:

```
mkdir /data/aces-listener-bitcoin/
```

```
spring:
  datasource:
    url: "jdbc:h2:/data/aces-listener-bitcoin/listener.db;DB_CLOSE_ON_EXIT=FALSE;AUTO_RECONNECT=TRUE"
    driver-class-name: "org.h2.Driver"
  jpa:
    hibernate:
      ddl-auto: "update"
```

Note: this configuration is intended to be uses for local development only. For production
environments, the listener application should be configured to use a `postgresql` database.


## Install Java Dependencies

Install maven dependencies (in the future, builds will be made available 
though an artifact repository):

```
cd ~
git clone git@github.com:ark-aces/ark-java-client.git
cd ark-java-client
mvn install
```

```
cd ~
git clone git@github.com:ark-aces/aces-server-lib-java.git
cd aces-server-lib-java
git fetch --all --tags --prune
git checkout tags/v5.6.0
mvn install
```


## Running Listener 

```
git clone https://github.com/ark-aces/aces-listener-bitcoin
cd aces-listener-bitcoin
mvn spring-boot:run -Dspring.config.location=file:/etc/aces-listener-bitcoin/application.yml
```


## Testing Listener

The listener API is now available at the following URL:

```
curl http://localhost:9090/
```
