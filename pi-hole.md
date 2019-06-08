# pi-hole

Instructions to install raspberry pi-hole. Before following these instructions, [set up](README.md) new raspberry pi. You need docker and docker compose to be installed.

## create pi-hole docker compose
Create `pihole.yaml`:
```
version: "3"

# https://github.com/pi-hole/docker-pi-hole/blob/master/README.md

services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp"
      - "80:80/tcp"
      - "443:443/tcp"
    environment:
      TZ: 'America/Chicago'
      # WEBPASSWORD: 'set a secure password here or it will be random'
    # Volumes store your data between container upgrades
    volumes:
       - './etc-pihole/:/etc/pihole/'
       - './etc-dnsmasq.d/:/etc/dnsmasq.d/'
    # run `touch ./var-log/pihole.log` first unless you like errors
    # - './var-log/pihole.log:/var/log/pihole.log'
    dns:
      - 127.0.0.1
      - 1.1.1.1
    # Recommended but not required (DHCP needs NET_ADMIN)
    #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
    cap_add:
      - NET_ADMIN
    restart: unless-stopped
```

## start and configure pi-hole
- run `docker-compose -f pihole.yaml up -d`
- exec inside pi-hole container `docker exec -it pihole /bin/bash`
- change password `pihole -a -p`

Configure dns to point to pi-hole. Web UI can be accessed on `http://<ip>/admin`.
