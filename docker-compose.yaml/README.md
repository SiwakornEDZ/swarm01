## ไฟล์ Docker Compose นี้กำหนดการใช้งานสอง services คือ "proxy" และ "backend" ดังนี้:

### 1.Service ชื่อ proxy:
* ใช้ Docker image ของ Nginx ที่เป็น official image จาก Docker Hub
* Mount ไฟล์ชื่อ ./proxy/nginx.conf จาก local machine ไปยังไฟล์ /etc/nginx/conf.d/default.conf ภายใน container โดยใช้โหมด read-only
* เปิด port 80 ของ container เพื่อให้สามารถเชื่อมต่อเข้าถึงได้ผ่าน port 80 ของ host machine
* ขึ้นอยู่กับการ start service ชื่อ backend ก่อน
### 2.Service ชื่อ backend:
* สร้าง Docker image โดยใช้ Dockerfile ที่อยู่ใน directory ชื่อ backend และระบุ target ชื่อ dev-envs สำหรับการ build
* Mount ไฟล์ Docker socket ที่อยู่บน host machine ชื่อ /var/run/docker.sock ไปยัง container เพื่อให้ container สามารถติดต่อกับ Docker daemon บน host machine ได้

### สรุป Docker Compose file ใช้กำหนดการทำงายของ service สองตัว ทำงานดังนี้ มี service หน้าที่เป็น proxy สำหรับ front-end ซึ่งใช้ Nginx และ service หลังที่เป็น back-end ซึ่ง build จาก Dockerfile และมีการ mount Docker socket เพื่อให้ container สามารถใช้งาน Docker daemon บน host machineได้โดยการรัน Docker Compose file นี้ด้วยคำสั่ง docker-compose up จะสร้าง container สองตัวตามการกำหนดของ service และเริ่มต้นใช้งานตามลำดับที่ถูกกำหนด

# อธิบาย Dockerfile
### Dockerfile นี้เป็นไฟล์สำหรับสร้าง Docker image ของแอปพลิเคชันที่เขียนด้วยภาษา Golang โดยมีขั้นตอนการสร้างดังนี้
* ใช้ base image ของ golang:1.18-alpine เพื่อสร้าง builder stage
* กำหนด working directory ไปที่ /code
* กำหนด environment variables เช่น CGO_ENABLED, GOPATH, GOCACHE
* คัดลอกไฟล์ go.mod และ go.sum เข้าไปใน container แล้วใช้คำสั่ง go mod download เพื่อดาวน์โหลด dependency ของโปรเจค
* คัดลอกไฟล์ทั้งหมดเข้าไปใน container
* ทำการ build แอปพลิเคชันด้วยคำสั่ง go build และนำ output ไปไว้ที่ไฟล์ bin/backend
* กำหนดคำสั่ง CMD เพื่อรัน application ที่ build ได้
* จากนั้นมี stage อีกหนึ่ง stage ชื่อว่า dev-envs ที่ใช้ builder stage เป็น base image 

ในขั้นตอนสุดท้าย นำ base image จาก scratch มาใช้ โดยมีไฟล์เพียงไฟล์เดียวคือไฟล์ backend จาก builder stage ที่อยู่ใน /code/bin/backend มาคัดลอกไปยัง /usr/local/bin/backend ใน image สุดท้าย และกำหนด CMD เพื่อรัน application ที่ build ได้

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
docker tag swarm01-backend siwakorn2345/swarm-backend:01
```
 
### push images ขึ้น docker hub และเพื่อเรียกใช้้งานใน dockercompose file

```
docker push siwakorn2345/swarm-backend:01
```
