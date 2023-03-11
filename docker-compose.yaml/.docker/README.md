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
