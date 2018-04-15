# Running Bitcoin-Ark Channel Services on Mainnet

This service node requires an Bitcoin Listener and Bitcoin JSON RPC service running.


## Set Up Dependencies

1. [ACES Service Server Setup](aces-service-server-setup.md)
2. [ACES Bitcoin Setup](aces-bitcoin-setup.md)
3. [Running Ark Listener](running-ark-listener-mainnet.md)


## Set up Bitcoin-Ark Channel Service

Create Service config file in `/etc/aces/aces-bitcoin-ark-channel-service/application.yml`:

```
serverInfo:
  name: "Aces BTC-ARK Channel Service"
  description: "ACES BTC to ARK Channel service for transferring BTC to ARK"
  instructions: >
    After this contract is executed, any BTC sent to depositBtcAddress will be exchanged for ARK and 
    sent directly to the given recipientArkAddress less service fees.
  version: "1.0.0"
  websiteUrl: "https://arkaces.com"
  flatFee: "0"
  percentFee: "1.00%"
  capacities:
    -
      value: "5000.00"
      unit: "ARK"
      displayValue: "5000 ARK"

spring:
  datasource:
    url: "jdbc:h2:/data/aces-bitcoin-ark-channel-service.db;DB_CLOSE_ON_EXIT=FALSE;AUTO_RECONNECT=TRUE"
    driver-class-name: "org.h2.Driver"
  jpa:
    hibernate:
      ddl-auto: "update"

arkNetworkConfigPath: "/etc/aces/.yml"

fees:
  btcFlatFee: 0
  btcPercentFee: 1

server:
  port: 9190

serviceArkAccount:
  address: change-me
  passphrase: change-me

bitcoinRpc:
  url: "http://localhost:18332/"
  username: "bitcoinrpc"
  password: "change_this_to_a_long_random_password"

bitcoinListener:
  url: http://localhost:9090

bitcoinEventCallbackUrl: http://localhost:9190/bitcoinEvents

```

Note: use bitcoinRpc username and password configured above for bitcoind.


Build the service application:

```
cd /apps/
git clone https://github.com/ark-aces/aces-bitcoin-ark-channel-service
cd aces-ark-bitcoin-channel-service
mvn package
find ./target -name "*.jar" -exec cp {} ./target/aces-bitcoin-ark-channel-service.jar \;
```


Create application systemd config in `/etc/systemd/system/aces-bitcoin-ark-channel-service.service`:

```
[Unit]
Description=Aces Bitcoin-Ark Channel Service

[Service]
Restart=always
WorkingDirectory=/apps/aces-bitcoin-ark-channel-service/target/
ExecStart=/usr/bin/java -Xms64m -Xmx128m -jar aces-bitcoin-ark-channel-service.jar \
  --spring.config.location=file:/etc/aces/aces-bitcoin-ark-channel-service/application.yml

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
systemctl enable aces-bitcoin-ark-channel-service
service aces-bitcoin-ark-channel-service start
```


Expose web service in nginx by adding a reverse proxy config to the web server config and restart:

```
server {
    # ....
    
    location /bitcoin-ark-channel/ {
         proxy_pass http://127.0.0.1:9190/;
    }
}
```

```
sudo service nginx restart
```

## Update Application

```
cd /apps/aces-bitcoin-ark-channel-service
git pull
mvn clean package
find ./target -name "*.jar" -exec cp {} ./target/aces-bitcoin-ark-channel-service.jar \;
sudo service aces-bitcoin-ark-channel-service restart
```