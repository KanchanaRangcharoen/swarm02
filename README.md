# swarm02 react-nginx
## Ref awaresome-compose
- https://github.com/docker/awesome-compose/tree/master/react-nginx
## Wakatime project share url 
- https://wakatime.com/@spcn08/projects/giqxvknufw
## URL-Deploy
- https://kan-react-nginx.xops.ipv9.me/

## Development Stage
**Ref Prepare Machine** => https://github.com/pitimon/dockerswarm-inhoure#readme

### Revert Proxy on Manager
- Set IP for Client

    - Edit file hosts
        - windows C:\Windows\System32\drivers\etc\hosts
        - Linux /etc/hosts
    - Add domain IP of manager for every application
        ```
        [ip manage] traefik.demo.local
        ```
    - [Step Revert Proxy by pitimon](https://github.com/pitimon/dockerswarm-inhoure/tree/main/ep03-traefik)
### Create Image
Bring selected apps push images 
- Compose up file compose.yaml to create images 
    ```
    docker compose up -d --build
    ```
- Login docker hub
    ```
    docker login  # user and password docker hub
    ```
- Create a tag that refers to images 
    ```
    docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
    ```

    **Ex.** docker tag react-nginx-frontend:latest kanchanarangcharoen/react-nginx:2904
- Upload an image to a repositories on docker hub
    ```
    docker push [OPTIONS] NAME[:TAG]
    ```
    **Ex.** docker push kanchanarangcharoen/react-nginx:2904

### Create docker-compose.yml
<details><summary><ins>SHOW CODE docker-compose.yml</ins></summary>
<p>


```
version: '3.3'
services:
  web:
    image: kanchanarangcharoen/react-nginx:2904
    networks:
     - webproxy
    logging:
      driver: json-file
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    deploy:
      replicas: 1
      labels:
        - traefik.docker.network=webproxy
        - traefik.enable=true
        - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
        - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
        - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
        - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
      resources:
        reservations:
          cpus: '0.1'
          memory: 10M
        limits:
          cpus: '0.4'
          memory: 50M
networks:
  webproxy:
    external: true
volumes:
  app:
```
</p>
</details>

### Bring docker-compose.yml Stack Deploy on the machine
![react](https://user-images.githubusercontent.com/119097660/224473048-27bb36e1-f0b6-45ef-9aea-df54d31af1b0.png)
