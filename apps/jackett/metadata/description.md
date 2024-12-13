
# JSON
```json
{
  "services": [
    {
      "name": "jackett",
      "image": "lscr.io/linuxserver/jackett:latest",
      "isMain": true,
      "internalPort": 9117,
      "environment": {
        "PUID": "1000",
        "PGID": "1000",
        "TZ": "${TZ}",
        "AUTO_UPDATE": "true"
      },
      "volumes": [
        {
          "hostPath": "${APP_DATA_DIR}/data",
          "containerPath": "/config"
        },
        {
          "hostPath": "${ROOT_FOLDER_HOST}/media/torrents",
          "containerPath": "/media/torrents"
        }
      ]
    }
  ]
} 
```
# YAML
```yaml
version: '3.7'
services:
  jackett:
    image: lscr.io/linuxserver/jackett:latest
    container_name: jackett
    environment:
    - PUID=1000
    - PGID=1000
    - TZ=${TZ}
    - AUTO_UPDATE=true
    dns:
    - ${DNS_IP}
    volumes:
    - ${APP_DATA_DIR}/data:/config
    - ${ROOT_FOLDER_HOST}/media/torrents:/media/torrents
    ports:
    - ${APP_PORT}:9117
    restart: unless-stopped
    networks:
    - tipi_main_network
    labels:
      traefik.enable: true
      traefik.http.middlewares.jackett-web-redirect.redirectscheme.scheme: https
      traefik.http.services.jackett.loadbalancer.server.port: 9117
      traefik.http.routers.jackett-insecure.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.jackett-insecure.entrypoints: web
      traefik.http.routers.jackett-insecure.service: jackett
      traefik.http.routers.jackett-insecure.middlewares: jackett-web-redirect
      traefik.http.routers.jackett.rule: Host(`${APP_DOMAIN}`)
      traefik.http.routers.jackett.entrypoints: websecure
      traefik.http.routers.jackett.service: jackett
      traefik.http.routers.jackett.tls.certresolver: myresolver
      traefik.http.routers.jackett-local-insecure.rule: Host(`jackett.${LOCAL_DOMAIN}`)
      traefik.http.routers.jackett-local-insecure.entrypoints: web
      traefik.http.routers.jackett-local-insecure.service: jackett
      traefik.http.routers.jackett-local-insecure.middlewares: jackett-web-redirect
      traefik.http.routers.jackett-local.rule: Host(`jackett.${LOCAL_DOMAIN}`)
      traefik.http.routers.jackett-local.entrypoints: websecure
      traefik.http.routers.jackett-local.service: jackett
      traefik.http.routers.jackett-local.tls: true
      runtipi.managed: true
 
```
