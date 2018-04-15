# Running Bitcoin Listener Mainnet


## Set up Bitcoin Network configuration

See: [ACES Bitcoin Setup](aces-bitcoin-setup.md)


## Set up Bitcoin Listener Application

### Setup Configuration

Create listener app config in `/etc/aces/aces-listener-bitcoin/application.yml`:

```
serverInfo:
  name: "AcesListenerBitcoin"
  description: "ACES Listener implementation for Bitcoin"
  version: "1.0.0"
  websiteUrl: "https://arkaces.com"
  
spring:
  datasource:
    url: "jdbc:h2:/data/aces/aces-listener-bitcoin.db;DB_CLOSE_ON_EXIT=FALSE;AUTO_RECONNECT=TRUE"
    driver-class-name: "org.h2.Driver"
  jpa:
    hibernate:
      ddl-auto: "update"

maxScanBlockDepth: 4

bitcoinRpc:
  username: bitcoinrpc
  password: change_this_to_a_long_random_password

server:
  port: 9090

arkAuth:
  requireAuth: false

logging:
  level:
    com.arkaces.aces_listener_bitcoin.BitcoinEventListener: ERROR
```

### Build application

```
cd /apps/
git clone https://github.com/ark-aces/aces-listener-bitcoin
cd aces-listener-bitcoin
mvn package -Djar.finalName=aces-listener-bitcoin
```

### Set up systemd

Create listener systemd config in `/etc/systemd/system/aces-listener-bitcoin.service`:

```
[Unit]
Description=Aces Listener Bitcoin

[Service]
Restart=always
WorkingDirectory=/apps/aces-listener-bitcoin/target/
ExecStart=/usr/bin/java -Xms64m -Xmx128m -jar aces-listener-bitcoin.jar \
  --spring.config.location=file:/etc/aces/aces-listener-bitcoin/application.yml

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
systemctl enable aces-listener-bitcoin
service aces-listener-bitcoin start
```
