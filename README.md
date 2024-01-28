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
