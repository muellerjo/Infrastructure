# Infrastructure

## MQTT Server Mosquitto (Docker)

https://github.com/sukesh-ak/setup-mosquitto-with-docker

Pull official Docker Image
```
docker pull eclipse-mosquitto
```

```
docker run -d -p 1883:1883 -p 9001:9001 --name mosquitto --restart always eclipse-mosquitto 
```


### Prepare Webserver for DNS / Domain

in etc/apache2/sites-available

nano muellerjonas-mqtt.conf

insert the following content

```
<VirtualHost *:80>

ServerName mqtt.muellerjonas.de

RewriteEngine on
RewriteCond %{SERVER_NAME} =cloud.mtwt.de
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```
save muellerjonas-mqtt.conf

in terminal execute:
```
sudo a2ensite muellerjonas-mqtt.con

systemctl reload apache2

sudo certbot --apache
```

then modify

```
<IfModule mod_ssl.c>
<VirtualHost *:443>

ServerName cloud.mtwt.de


# Proxy to Portainer
ProxyPreserveHost On
ProxyPass / http://127.0.0.1:49158/
ProxyPassReverse / http://127.0.0.1:49158/
RequestHeader set X-Forwarded-Proto "https"

ProxyVia Block

<Proxy *>
Require all granted
</Proxy>




RewriteEngine on
# Some rewrite rules in this file were disabled on your HTTPS site,
# because they have the potential to create redirection loops.

# RewriteCond %{SERVER_NAME} =cloud.mtwt.de
# RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]

SSLCertificateFile /etc/letsencrypt/live/cloud.mtwt.de/fullchain.pem
SSLCertificateKeyFile /etc/letsencrypt/live/cloud.mtwt.de/privkey.pem
Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
</IfModule>
```

Configure Mosquitto in Homeassistant in order to receive MQTT-messages

```
connection bridge1
address eu1.cloud.thethings.network:1883
start_type automatic
topic v3/# in 2
local_clientid ttn-climate.mosquitto
remote_username <user>
remote_password <pwd>
try_private false
#bridge_insecure false
cleansession true

connection bridge2
address 173.249.xxx.xxx:1883
start_type automatic
# if you need to have all topics without a filter
topic # in 0 "" ""
local_clientid homassistant
remote_username <>
remote_password <>
try_private false
#bridge_insecure false
cleansession true
```
