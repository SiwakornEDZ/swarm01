# อธิบาย docker-compose.yaml

### เป็นไฟล์ docker-compose ที่ใช้งาน traefik เป็น load balance และ revert proxy
### frontend กำหนดโดยใช้ image ของ dockerfile ทำให้ traefik โดยกำหนด port 80 ตรวจหาและเชื่อมต่อไปที่ comtainer ของ dockerในเครือข่ายที่ตั้งไว้
### เพื่อให้ traefik สามารถค้นหา container ของ docker ได้ frontend จึงได้เชื่อมต่อกับ Docker socket (/var/run/docker.sock:/var/run/docker.sock) 
### frontend จะขึ้นอยู่กับ backend ซึ่งถูกสร้างจาก Dockerfile ที่อยู่ใน dir/backend backend เปิดใช้งานอยู่ 2ตัวคือ 1.ใช้สำหรับเปิดใช้บริการ traefik
### 2.ใช้กำหนดการเชื่อมต่อสำหรับ frontend ใน directory
### 3.ใช้กำหนด port ให้บริการ

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

## docker depoly

```
sudo docker compose up -d
```

### กำหนด tag ของ images ที่ต้องการ

```
docker tag linuxserver/plex:lastest siwakorn2345/plex:sw06
```
 
### push images ขึ้น docker hub และเพื่อเรียกใช้้งานใน dockercompose file

```
docker push siwakorn2345/plex:sw06
```
