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
