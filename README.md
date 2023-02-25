<p align="center">
  <a href="https://github.com/creatif-studio/dockertls">
    <img alt="Docker TLS" width="75px" height="75px" src="./assets/logo.png">
  </a>
</p>

<p align="center">
  Generate TLS Certificates for Securing the Docker Daemon Socket
</p>

## How to generate certificate?

You can run this command line by line

```
git clone https://github.com/creatif-studio/dockertls.git
cd dockertls; chmod +x generate.sh
sudo ./generate.sh

# note:
# copy ca.pem,server-cert.pem,server-key.pem
# insert all files into docker servers in this location `/data/certs/`
```

**Example usage**

```
./generate.sh -m ca -pw change-your-ramdon-string -t certs -e 900
./generate.sh -m server -h server -pw change-your-ramdon-string -t certs -e 900
./generate.sh -m client -h client -pw change-your-ramdon-string -t certs -e 900

# note:
# -h  : hosts
# -pw : password
```

## How to enable Docker TLS?

- Open your docker.service `/lib/systemd/system/docker.service`
- Look this line `ExecStart=/usr/bin/dockerd -H fd://`
- Comment from `# -H fd:// ...`
- Restart your daemon service `systemctl daemon-reload`
- Restart your docker service `systemctl restart docker`
- Create a new file in `/etc/docker/daemon.json` in your docker servers
- Look `daemon.json` in this repository
- Restart your docker service `systemctl restart docker`
- Create virtual host `"echo your-ip server" > /etc/hosts`

## How to use?

```
docker -H server:2376 --tlsverify --tlscacert=ca.pem --tlscert=client-cert.pem --tlskey=client-key.pem ps
```
