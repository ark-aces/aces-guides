# ACES Node Server Setup

This guide tells you how to set up a secure ACES Service node server using Ubuntu 16.04.


## Overview

ACES Service nodes run different blockchains and dependency services depending on the 
services provided by the node. Every ACES Service node exposes a number of web services
over http. Each service runs as a Java Spring Boot microservice fronted by an nginx
reverse proxy to the microservice port. 


## General Security

To better secure your server, you should disable username and password authentication and only 
allow SSH key authentication for a non-root user.

- https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04


## Setup Firewall

```
sudo apt-get install ufw
sudo ufw allow ssh
sudo ufw allow http
sudo ufw enable
```


# Install Dependencies

```
sudo apt-get instal nginx

sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update -y
sudo apt-get install oracle-java8-installer

sudo apt-get install maven
```


# Install SSL Certificates

We recommend using LetsEncrypt for free SSL certificates. 

- https://letsencrypt.org/


## Install Dependencies

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

## Generating Certificates

Set DOMAIN below to a domain name registered to your server IP address. LetsEncrypt will start
a local 

First temporarily disable nginx server to allow LetsEncrypt to validate your domain and issue
the SSL certs.  

```
sudo service nginx stop
```

```
export DOMAIN_NAME=example.com
certbot certonly -d $DOMAIN_NAME --force-renewal
```

## Set Nginx

Edit `/etc/nginx/sites-enabled/default` and add the following basic setup. Replace {{domain}} with 
your actual aces node domain name.

```
server {
	listen 80;
	server_name {{domain}};
	return 301 https://{{domain}}$request_uri;
}

server {
    listen 443 ssl;

	server_name {{domain}};

	ssl_certificate /etc/letsencrypt/live/{{domain}}/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/{{domain}}/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
}
```
