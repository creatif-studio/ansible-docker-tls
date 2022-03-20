# Docker API
Access your docker on servers from localhost like using kubectl.

## How to generate Docker API Certs?
- Clone this repository
- Change permission `chmod +x generate.sh`
- Generate your certs with `generate.sh`
- Copy `ca.pem`, `server-cert.pem`, `server-key.pem` to your docker servers in this location `/data/certs/`

**Example usage**
```
./generate.sh -m ca -pw change-your-ramdon-string -t certs -e 900
./generate.sh -m server -h server -pw change-your-ramdon-string -t certs -e 900
./generate.sh -m client -h client -pw change-your-ramdon-string -t certs -e 900
```

## How to enable Docker API?
- Open your docker.service `/lib/systemd/system/docker.service`
- Look this line `ExecStart=/usr/bin/dockerd -H fd://`
- Comment from `-H fd:// ...`
- Restart your daemon service `systemctl daemon-reload`
- Restart your docker service `systemctl restart docker`
- Create a new file in `/etc/docker/daemon.json` in your docker servers
- Look `daemon.json` in this repository
- Restart your docker service `systemctl restart docker`
- Create virtual host `"echo your-ip server" > /etc/hosts`

## How to use?
- `docker -H server:2376 --tlsverify --tlscacert=ca.pem --tlscert=client-cert.pem --tlskey=client-key.pem ps -a`
