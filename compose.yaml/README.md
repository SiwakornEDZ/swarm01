# โค้ดนี้เป็นไฟล์ Docker Compose ซึ่งใช้สำหรับสร้างและกำหนดค่า Docker containers ที่ต้องการ depoly 

### ในส่วนแรกส่วนของ services เรากำหนด Docker container ที่จะต้องรันคือ container ชื่อ test-volume โดยจะใช้ image จาก docker hub ชื่อ siwakorn2345/swarm01-backend:01 เป็น base image ในการสร้าง container นี้
* แล้วก็กำหนด network ที่ container นี้จะ join คือ network ชื่อ webproxy ที่เป็น external network
* ส่วนของ logging เป็นการกำหนดว่า container นี้จะใช้ logging driver เป็น json-file ในการเก็บ log
* ส่วน deploy เป็นการกำหนดการ deploy container นี้ ในที่นี้จะ deploy แค่ 1 replica และกำหนด label ของ Traefik 
* traefik.docker.network=webproxy คือการบอก Traefik ว่า network ของ container นี้คือ webproxy
* traefik.http.routers.whale0011-https.entrypoints=websecure คือการบอก Traefik ว่า router นี้จะใช้ entrypoint เป็น websecure ซึ่งเป็น entrypoint ที่เชื่อมต่อกับ HTTPS
* traefik.http.routers.whale0011-https.rule=Host("whale0011.xops.ipv9.me") คือการบอก Traefik ว่าเมื่อมี request มาที่ whale0011.xops.ipv9.me ให้ใช้ router นี้ในการ handle request
* traefik.http.routers.whale0011-https.tls.certresolver=default คือการบอก Traefik ว่าใช้ certresolver ชื่อ default ในการดึง SSL/TLS certificate สำหรับ HTTPS connection
* traefik.http.services.whale0011.loadbalancer.server.port=80 คือการบอก Traefik ว่า service ที่เชื่อมต่อกับ router นี้จะใช้ port ที่เป็น HTTP (port 80)
