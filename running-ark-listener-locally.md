# Running Ark Listener Locally

## Set up Ark Network configuration

The listener reads transactions off only trusted Ark nodes. For your listener set up
you can either trust the public ACES Ark Node instance or run 
[ark-node](https://github.com/ArkEcosystem/ark-node) locally and use 
that as a trusted node.


### Trusting ACES Listener Node instance

Set up ark network configuration to point to ACES public Listener node
ark-listener-mainnet.arkaces.com
in `/etc/aces-ark-listener/ark-network.yml`:

```
scheme: http
version: 1.0
netHash: 6e84d08bd299ed97c212c886c98a57e36545c8f5d645ca7eeae63a8bd62d8988
seedPeers:
  -
    hostname: 52.35.9.191
    port: 4001
trustedPeers:
  -
    hostname: 52.35.9.191
    port: 4001
```


### Trusting Local Ark Node instance

Set up ark network configuration to point to your local ark-node instace
in `/etc/aces-ark-listener/ark-network.yml`:

```
scheme: http
version: 1.0
netHash: 6e84d08bd299ed97c212c886c98a57e36545c8f5d645ca7eeae63a8bd62d8988
seedPeers:
  -
    hostname: localhost
    port: 4001
trustedPeers:
  -
    hostname: localhost
    port: 4001
```


## Set up ACES Ark Listener configuration

Create listener app config in `/etc/aces-listener/application.yml`:

```
serverInfo:
  name: "LocalArkListener"
  description: "Local Ark Listener node"
  version: "1.0.0"

arkNetworkConfigPath: "file:/etc/aces-listener/ark-network.yml"

scanTransactionDepth: 500

server:
  port: 9091

arkAuth:
  requireAuth: false 
```


## Install dependencies

Install maven dependencies manually (in the future, builds will be made available 
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
mvn install
```


## Running Listener 

```
git clone https://github.com/ark-aces/aces-listener-ark
cd aces-listener-ark
mvn spring-boot:run -Dspring.config.location=file:/etc/aces-listener/application.yml
```


## Testing Listener

```
curl http://localhost:9091/
```
