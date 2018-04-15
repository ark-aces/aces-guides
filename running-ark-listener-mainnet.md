# Running Ark Listener Mainnet

## Set up Ark Network configuration

The listener reads transactions off only trusted Ark nodes. For your listener set up
you can either trust the public ACES Ark Node instance or run 
[ark-node](https://github.com/ArkEcosystem/ark-node) locally and use 
that as a trusted node.


### Trusting ACES Listener Node instance

Set up ark network configuration to point to ACES public Listener node
ark-listener-mainnet.arkaces.com
in `/etc/aces/ark-network.yml`:

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
```


### Trusting Local Ark Node instance (optional)

Set up ark network configuration to point to your local ark-node instance
in `/etc/aces/ark-network.yml`:

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


## Set up ACES Ark Listener Application

### Setup Configuration

Create listener app config in `/etc/aces/aces-listener-ark/application.yml`:

```
serverInfo:
  name: "ArkListener"
  description: "Ark Listener node"
  version: "1.0.0"

arkNetworkConfigPath: "file:/etc/aces/ark-network.yml"

scanTransactionDepth: 500

server:
  port: 9091

arkAuth:
  requireAuth: false 
```

### Build application

```
cd /apps/
git clone https://github.com/ark-aces/aces-listener-ark
cd aces-listener-ark
mvn package -Djar.finalName=aces-listener-ark
```

### Set up systemd

Create listener systemd config in `/etc/systemd/system/aces-listener-ark.service`:

```
[Unit]
Description=Aces Listener Ark

[Service]
Restart=always
WorkingDirectory=/apps/aces-listener-ark/target/
ExecStart=/usr/bin/java -Xms64m -Xmx128m -jar aces-listener-ark.jar \
  --spring.config.location=file:/etc/aces/aces-listener-ark/application.yml

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
systemctl enable aces-listener-ark
service aces-listener-ark start
```


## Testing Listener

```
curl http://localhost:9091/
```
