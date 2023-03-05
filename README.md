# Ref awaresome-compose
- From https://github.com/docker/awesome-compose/tree/master/apache-php
# Wakatime project share url 
- https://wakatime.com/@spcn08/projects/giqxvknufw
## ขั้นตอนการพัฒนา
### push images เข้า docker hub
1. $ docker compose up -d --build เพื่อสร้าง images
2. $ docker ps เพื่อเช็คimages
3. $ docker login เพื่อเข้า docker hub
4. $ docker tag swarm02-web kanchanarangcharoen/swarm02-web:0503
5. $ docker push kanchanarangcharoen/swarm02-web:0503 เพื่อ push images เข้า docker hub
