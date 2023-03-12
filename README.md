# REF

- [https://github.com/docker/awesome-compose/tree/master/nginx-golang](https://github.com/docker/awesome-compose/tree/master/nginx-golang)

# Wakatime

- https://wakatime.com/@spcn21/projects/ziszqdtyet

# url(plex)

- https://whale0011.xops.ipv9.me

# ขั้นตอนการติดตั้ง
<details>
  <summary> กดเพื่อดูขั้นตอนการติดตั้ง vm docker nala package และคำสั่งหลัง clone</summary>
  
## เตรียมมการติดตั้ง vm สำหรับการทำ manager และ 2 swarmnode 
## สเปค cpu 2 core ram 2GB disk 32 network ipv4 DHCP ipv6 static กำหนด ssh key
## คำสั่งเข้าสิทธิ  root 

```
sudo -i
```

## เปลี่ยนชื่อ hostname 

```
hostnamectl set-hostname “Change name”
```

## ติดตั้ง docker

```
apt update ; apt upgrade -y
```

```
apt-get install \
    ca-certificates \
    curl wget \
    gnupg \
    lsb-release -y

```

```
mkdir -m 0755 -p /etc/apt/keyrings
```

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```
apt-get update
```

```
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```

## ติดตั้ง nala package management แทน apt package management


```
wget https://gitlab.com/volian/nala/uploads/605d833bdffd23cee4bb6670b2d6c27b/nala_0.12.1_all.deb
```

```
dpkg -i nala_0.12.1_all.deb 
```

```
apt-get -f install -y 
```

```
tee -a /etc/apt/sources.list.d/nala-sources.list <<EOF 
```

```
deb https://mirror1.ku.ac.th/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.nipa.cloud/ubuntu/ jammy main restricted universe multiverse
deb https://mirror.kku.ac.th/ubuntu/ jammy main restricted universe multiverse
deb http://mirror1.totbb.net/ubuntu/ jammy main restricted universe multiverse
EOF
```

```
nala update  
```

```
nala upgrade -y 
```

```
nala list —upgradable
```

```
nala install htop dnsutils mtr -y
```

```
reboot
```

## ทำการโคลน vm สำหรับการใช้สร้าง vm manager swarm vm swarm1 vm swarm2

```
cp /dev/null /etc/machine-id
```

```
rm /var/lib/dbus/machine-id
```

```
ln -s /etc/machine-id /var/lib/dbus/machine-id
```

```
init 0
```

## ให้สิทธิผู้ใช้งานกับ docker

```
sudo usermod -aG docker $USER
```

```
docker ps
```
  
</details>



## code compose บนคลัสเตอร์ xops

```
version: '3.3'
services:
  test-volume:
    image: siwakorn2345/swarm01-backend:01
    networks:
     - webproxy
    logging:
      driver: json-file
    deploy:
      replicas: 1
      labels:
        - traefik.docker.network=webproxy
        - traefik.enable=true
        - traefik.http.routers.whale0011-https.entrypoints=websecure
        - traefik.http.routers.whale0011-https.rule=Host("whale0011.xops.ipv9.me")
        - traefik.http.routers.whale0011-https.tls.certresolver=default
        - traefik.http.services.whale0011.loadbalancer.server.port=80
      resources:
        reservations:
          cpus: '0.1'
          memory: 6M
        limits:
          cpus: '0.4'
          memory: 50M
networks:
  webproxy:
    external: true
```
### อธิบาย compose ที่ใช้ในการ deploy บนคลัสเตอร์ xops

- [https://github.com/SiwakornEDZ/swarm01/tree/master/compose.yaml](https://github.com/SiwakornEDZ/swarm01/tree/master/compose.yaml)

### อธิบาย dockercompose.yaml

- [https://github.com/SiwakornEDZ/swarm01/tree/master/docker-compose.yaml/.docker](https://github.com/SiwakornEDZ/swarm01/tree/master/docker-compose.yaml/.docker)

### อธิบาย compose.yaml และ Dockerfile

- [https://github.com/SiwakornEDZ/swarm01/blob/master/docker-compose.yaml/README.md](https://github.com/SiwakornEDZ/swarm01/blob/master/docker-compose.yaml/README.md)

# Link to images on xops.ipv9.me

- https://whale0011.xops.ipv9.me/

# Ref example code 

- https://github.com/pitimon/dockerswarm-inhoure?fbclid=IwAR05dUk05KjPAWSfDZs0ecF9RjuXh6w86rTt2IFKUQrwSrFopYEgTj-x6dY
 







