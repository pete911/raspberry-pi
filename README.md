# pihole

instructions to install raspberry pihole

## download rasbian lite
 - download rasbian lite
 - install with etcher
 - create empty 'ssh' file in boot partion (to enable ssh server)

## install docker and docker compose
ssh to raspbarry pi (ssh should be enabled from the previous step), default username is `ip` and password is `raspberry`.
Follow instructions after ssh to change password (or create and use ssh key)

 - [install docker](https://howchoo.com/g/nmrlzmq1ymn/how-to-install-docker-on-your-raspberry-pi)
   - `curl -sSL https://get.docker.com | sh`
 - install docker-compose
   - `sudo apt-get update`
   - `sudo apt-get install libffi-dev`
   - `sudo apt-get install -y python python-pip`
   - `sudo pip install docker-compose`
   - `sudo shutdown -r now`
   - ssh back and test: `docker run hello-world`

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

run `docker-compose -f pihole.yaml up -d`
