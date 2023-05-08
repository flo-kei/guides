# Wireguard Setup Guide

This guide illustrates one possible way of setting up a wireguard server 
and client using docker-compose and wg-quick.

Requirements for completing this guide:
 - a distro of your choice
 - docker & docker-compose installed on the server
 - wireguard installed on the client
 - basic linux knowledge
 - 15min or up to 2 days if nothing works for some reason ¯\\_(ツ)_/¯

## Server setup:

Most information on how to setup a docker-compose server can be found here:
https://github.com/linuxserver/docker-wireguard

Copy the docker-compose.yml from the README.md.

These are the only settings I adjusted in the docker-compose.yml (for 2 users):

```
environment:
    - PEERS=2
volumes:
    - ./config:/config
```

## Client setup:

### Get client-config from server
The config for the first user can be found at "./config/peer1/peer1.conf".


#### Connect a mobile device:
The directory "./config/peer1/" also contains a png file with a QR 
code which can be used to connect using a mobile device.

(Apps are available for both Android & iOS:

https://play.google.com/store/apps/details?id=com.wireguard.android&hl=de&pli=1

https://apps.apple.com/de/app/wireguard/id1441195209
)

When using a mobile device, none of the below listed steps need 
to be applied, as the app contains a QR-code scanner for 
importing the config.

#### Get configs via docker exec:
(this bit can be ignored by most people)

You can get the configs directly from the container, 
if you don't like bind-mounts.
In this case use:
``` 
docker exec -it wireguard bash
```
to get a bash shell within the container.

### Copy config to client
The file should be located in "etc/wireguard" 
and it should be named the same as the interface (e.g. wg0.conf)

### Set permissions for file
Set permissions of wg0.conf to 077
```
chmod 077 wg0.conf
```

### Try it out

Open connection:
```
wg-quick up wg0
```

Close connection:
```
wg-quick down wg0
```

I also wrote a small script to make opening and closing the connection easier:
[vpn](./vpn)
