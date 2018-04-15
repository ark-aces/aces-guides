# Running Ark-Bitcoin Channel Services on Mainnet

This service node requires an Ark Listener and Bitcoin JSON RPC service running.


## Set Up Dependencies

1. [ACES Service Server Setup](aces-service-server-setup.md)
2. [ACES Bitcoin Setup](aces-bitcoin-setup.md)
3. [Running Ark Listener](running-ark-listener-mainnet.md)


## Set up Ark-Bitcoin Channel Service

Create Service config file in `/etc/aces/aces-ark-bitcoin-channel-service/application.yml`:

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

arkNetworkConfigPath: "file:/etc/aces/ark-network.yml"

spring:
  datasource:
    url: "jdbc:h2:/data/aces/aces-ark-bitcoin-channel-service.db;DB_CLOSE_ON_EXIT=FALSE;AUTO_RECONNECT=TRUE"
    driver-class-name: "org.h2.Driver"
  jpa:
    hibernate:
      ddl-auto: "update"

fees:
  btcFlatFee: 0.005
  btcPercentFee: 2.25

server:
  port: 9191

bitcoinRpc:
  url: http://127.0.0.1:8332/
  username: bitcoinrpc
  password: change_this_to_a_long_random_password
  
arkListener:
  url: http://localhost:9091

arkEventCallbackUrl: http://localhost:9191/arkEvents
```

Note: use bitcoinRpc username and password configured above for bitcoind.


Build the service application:

```
cd /apps/
git clone https://github.com/ark-aces/aces-ark-bitcoin-channel-service
cd aces-ark-bitcoin-channel-service
mvn package
find ./target -name "*.jar" -exec cp {} ./target/aces-ark-bitcoin-channel-service.jar \;
```


Create application systemd config in `/etc/systemd/system/aces-ark-bitcoin-channel-service.service`:

```
[Unit]
Description=Aces Ark Bitcoin Channel Service

[Service]
Restart=always
WorkingDirectory=/apps/aces-ark-bitcoin-channel-service/target/
ExecStart=/usr/bin/java -Xms64m -Xmx128m -jar aces-ark-bitcoin-channel-service.jar \
  --spring.config.location=file:/etc/aces/aces-ark-bitcoin-channel-service/application.yml

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
systemctl enable aces-ark-bitcoin-channel-service
service aces-ark-bitcoin-channel-service start
```


Expose web service in nginx by adding a reverse proxy config to the web server config and restart:

```
server {
    # ....
    
    location /ark-bitcoin-channel/ {
         proxy_pass http://127.0.0.1:9191/;
    }
}
```

```
sudo service nginx restart
```

## Update Application

```
cd /apps/aces-ark-bitcoin-channel-service
git pull
mvn clean package
find ./target -name "*.jar" -exec cp {} ./target/aces-ark-bitcoin-channel-service.jar \;
sudo service aces-ark-bitcoin-channel-service restart
```