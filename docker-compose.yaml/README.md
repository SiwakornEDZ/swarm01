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
