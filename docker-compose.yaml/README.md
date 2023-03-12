# อธิบาย compose.yaml

### เป็นไฟล์ docker-compose ที่ใช้งาน traefik เป็น load balance และ revert proxy
### frontend กำหนดโดยใช้ image ของ dockerfile ทำให้ traefik โดยกำหนด port 80 ตรวจหาและเชื่อมต่อไปที่ comtainer ของ dockerในเครือข่ายที่ตั้งไว้
### เพื่อให้ traefik สามารถค้นหา container ของ docker ได้ frontend จึงได้เชื่อมต่อกับ Docker socket (/var/run/docker.sock:/var/run/docker.sock) 
### frontend จะขึ้นอยู่กับ backend ซึ่งถูกสร้างจาก Dockerfile ที่อยู่ใน dir/backend backend เปิดใช้งานอยู่ 3ตัวคือ 
### 1.ใช้สำหรับเปิดใช้บริการ traefik
### 2.ใช้กำหนดการเชื่อมต่อสำหรับ frontend ใน directory
### 3.ใช้กำหนด port ให้บริการ

# อธิบาย dockerfile
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
