# REF

- [https://github.com/docker/awesome-compose/tree/master/nginx-golang](https://github.com/docker/awesome-compose/tree/master/nginx-golang)

# Wakatime

- https://wakatime.com/@spcn21/projects/ziszqdtyet

#url(plex)

- https://whale0011.xops.ipv9.me

## เตรีมการติดตั้ง vm สำหรับการทำ manager และ 2 swarmnode 
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

## Swarm init (ติดตั้ง swarm บน manager node)

```
docker swarm init
```
### copy token code ไปใช้งานที่ swarm1 และ swarm2
### เมื่อใช้้งานคำสั่ง token แล้วให้ตรวจเช็คโดยใช้คำสั่ง

```
docker node ls
```

![Image 12-3-2566 BE at 04 43](https://user-images.githubusercontent.com/87377798/224512757-06270fff-e6e9-4edb-b86c-d3bfa97d16ab.jpg)

## ติดตั้ง  portainer ใน (manager node)

```
curl -L https://downloads.portainer.io/ce2-17/portainer-agent-stack.yml -o portainer-agent-stack.yml
```

```
docker stack deploy -c portainer-agent-stack.yml portainer
```

## config ip ใน hosts files 


### Windows client C:\Windows\System32\drivers\etc\hosts
### Linux/Mac client /etc/hosts

![Image 6-3-2566 BE at 01 45](https://user-images.githubusercontent.com/87377798/222979641-a1aa0a98-96e6-4ffb-991d-a4bf90c72b3f.jpg)

## ติดตั้ง traefik

### เตรียม floder ชื่อว่า taefik 
### สร้างไฟล์ traefik-host.yml

### Step

### สร้าง network 

```
docker network create --driver=overlay traefik-public
```

### สร้าง tag ในโหนดเพื่อเก็บค่าไฟล์

```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
echo $NODE_ID
```

```
docker node update --label-add traefik-public.traefik-public-certificates=true $NODE_ID
```

### กำหนดค่า email,domain,username,password 

```
export EMAIL=user@smtp.com
export DOMAIN=traefik.cpedemo.local
export USERNAME=admin
export PASSWORD=changeMe
export HASHED_PASSWORD=$(openssl passwd -apr1 $PASSWORD)
echo $HASHED_PASSWORD

```
### ติดตั้ง traefix ใน stack ของ portainer

```
docker stack deploy -c traefik-host.yml traefik
```

### ทดสอบโดยการกดลิงค์

- traefik.cpedemo.local 

![Image 12-3-2566 BE at 04 48](https://user-images.githubusercontent.com/87377798/224513418-7d9ebf28-42b2-4e2a-8e46-8fa2b748dc24.jpg)

## ติดตั้ง swarmpit 

### ขั้นตอน 
 
### กำหนดค่า โดเมน

```
export DOMAIN=swarmpit.cpedemo.local
```

### กำหนดค่า  lable เพื่อใช้สำหรับ CouchDB database

```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
```
```
docker node update --label-add swarmpit.db-data=true $NODE_ID
```

### กำหนดค่า  lable เพื่อใช้สำหรับ Influx database

```
export NODE_ID=$(docker info -f '{{.Swarm.NodeID}}')
```

```
docker node update --label-add swarmpit.influx-data=true $NODE_ID
```

### deploy stack swarmpit

```
docker stack deploy -c swarmpit.yml swarmpit
```

### check stack ด้วยคำสั่งนี้

```
docker stack ps swarmpit
```

```
docker service logs swarmpit_app
```
### ตรวจเช็คโดย

- http://swarmpit.cpedemo.local

![Image 12-3-2566 BE at 04 48](https://user-images.githubusercontent.com/87377798/224512903-a91bc888-ae17-45ae-b51a-32f1ee07ce44.jpg)

## สร้าง image สำหรับการเตรียม push ขึ้น dockerhub

### ขั้นตอนแรก

### ตรวจเช็ค image ใน docker โดยใช้คำสั่ง

```
docker images
```

### ขั้นตอนที่ 2

### login docker โดยใช้  username และ password

```
docker login
```
### ขั้นตอนที่ 3 

### กำหนด tag ของ images ที่ต้องการ

```
docker tag swarm02-web siwakorn2345/swarm02-web:01
```
 
### push images ขึ้น docker hub และเพื่อเรียกใช้้งานใน dockercompose file

```
docker push siwakorn2345/swarm02-web:01
```

### กำหนดค่าไฟล์ compose.yaml เพื่อนำไป deploy stack ใน portainer
## ยังไม่สมบูรณ์

```
version: '3.7'

services:
  db:
    image: postgres
    environment:
      postgres_user: postgres
      postgres_password: postgres
    networks:
      - default
    volumes:
      - db_data:/var/lib/postgresql/data
  web:
    image: siwakorn2345/swarm02-web:01
    networks:
      - traefik-public
      - default
    volumes:
      - static_data:/usr/src/app/static
    ports:
      - "8080:8080"
    depends_on:
      - db
    deploy:
      replicas: 4
      labels:
        - traefik.docker.network=traefik-public
        - traefik.enable=true
        - traefik.http.routers.${appname}-https.entrypoints=websecure
        - traefik.http.routers.${appname}-https.rule=Host("{appname}.xops.ipv9.me")
        - traefik.http.routers.${appname}-https.tls.certresolver=default
        - traefik.http.services.${appname}.loadbalancer.server.port=8080

      restart_policy:
        condition: any
      update_config:
        delay: 5s
        parallelism: 1
        order: start-first
volumes:
  db_data:
  static_data:

networks:
  default:
    driver: overlay
    attachable: true   
  traefik-public:
    external: true
```

## ยังไม่สมบูรณ์

# Ref example code 

- https://github.com/pitimon/dockerswarm-inhoure?fbclid=IwAR05dUk05KjPAWSfDZs0ecF9RjuXh6w86rTt2IFKUQrwSrFopYEgTj-x6dY
 







