# raspberry pi setup

## download rasbian lite
 - download rasbian lite
 - install with etcher
 - create empty 'ssh' file in boot partion (to enable ssh server)

## setup
SSH to raspberry pi (default username `pi` and password `raspberry`)
 - change password `passwd`
 - change hostname
   - `sudo hostname <new_hostname>`
   - `sudo vi /etc/hostname` change all 'raspberry' to new hostname
   - `sudo vi /etc/hosts` change all 'raspberry' to new hostname
 - update `sudo apt-get update && sudo apt-get upgrade -y`

## ssh
Locally (we name the keys with the same name as the new hostname wit set in the previous steps).
Replace `<hostname>` and `<pi_IP>` in the instructions.
 - `cd ~/.ssh`
 - create ssh keys `ssh-keygen -f <hostname>`
 - copy public key to raspberry pi `ssh-copy-id -i <hostname>.pub pi@<pi_IP>`
 - add ssh config entry `vi config`
```
Host <hostname>
   Hostname <pi_IP>
   User pi
   IdentityFile ~/.ssh/<hostname>
```

## install docker and docker compose

 - [install docker](https://howchoo.com/g/nmrlzmq1ymn/how-to-install-docker-on-your-raspberry-pi)
   - `curl -sSL https://get.docker.com | sh`
   - `sudo usermod -aG docker pi` (you need to logout and log back in after this step)
 - install docker-compose
   - `sudo apt-get update`
   - `sudo apt-get install libffi-dev`
   - `sudo apt-get install -y python python-pip`
   - `sudo pip install docker-compose`
   - `sudo reboot`
   - ssh back and test: `docker run hello-world`
