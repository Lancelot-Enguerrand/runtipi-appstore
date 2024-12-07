
# JSON
```json
{
  "services": [
    {
      "name": "baikal",
      "image": "ckulka/baikal:0.10.1-nginx",
      "isMain": true,
      "internalPort": 80,
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/config",
          "containerPath": "/var/www/baikal/config"
        },
        {
          "hostPath": "${APP_DATA_DIR}/specific",
          "containerPath": "/var/www/baikal/Specific"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3'
services:
  baikal:
    container_name: baikal
    image: ckulka/baikal:0.10.1-nginx
    restart: unless-stopped
    ports:
    - ${APP_PORT}:80
    volumes:
    - ${APP_DATA_DIR}/config:/var/www/baikal/config
    - ${APP_DATA_DIR}/specific:/var/www/baikal/Specific
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.baikal-web-redirect.redirectscheme.scheme: https
      traefik.http.services.baikal.loadbalancer.server.port: 80
      traefik.http.routers.baikal-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.baikal-insecure.entrypoints: web
      traefik.http.routers.baikal-insecure.service: baikal
      traefik.http.routers.baikal-insecure.middlewares: baikal-web-redirect
      traefik.http.routers.baikal.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.baikal.entrypoints: websecure
      traefik.http.routers.baikal.service: baikal
      traefik.http.routers.baikal.tls.certresolver: myresolver
      traefik.http.routers.baikal-local-insecure.rule: Host(`baikal.${LOCAL_DOMAIN}`)
      traefik.http.routers.baikal-local-insecure.entrypoints: web
      traefik.http.routers.baikal-local-insecure.service: baikal
      traefik.http.routers.baikal-local-insecure.middlewares: baikal-web-redirect
      traefik.http.routers.baikal-local.rule: Host(`baikal.${LOCAL_DOMAIN}`)
      traefik.http.routers.baikal-local.entrypoints: websecure
      traefik.http.routers.baikal-local.service: baikal
      traefik.http.routers.baikal-local.tls: true
      runtipi.managed: true
 
```
